---
layout: post
title: "[SPRING] 엑셀 업로드 및 다운로드 구현(POI 라이브러리)"
comments: true
description: ""
keywords: ""
tags: [SPRING]
---

# Spring 엑셀 업로드 및 다운로드
​
```
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<html>
<head>
	<title>Excel 업로드 및 다운로드</title>
	<script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
</head>
<body>
<script type="text/javascript">
	function excelUploadProcess(){
		var f = new FormData(document.getElementById('form1'));
		$.ajax({
			url: "uploadExcel",
			data: f,
			processData: false,
			contentType: false,
			type: "POST",
			success: function(data){
				console.log(data);
				document.getElementById('result').innerHTML = JSON.stringify(data);
			}
		});
	}
    
	function excelDownloadProcess(){
		document.form1.target = "hide_frame";
		document.form1.action = "excelDownload";
		..
		document.form1.submit();
	}
</script>
<form id="form1" name="form1" method="post" enctype="multipart/form-data" onsubmit="return false">
	<input type="file" id="fileInput" name="fileInput">
	<a href="javascript:void(0);" onclick="excelUploadProcess()">엑셀업로드</a>
	<a href="javascript:void(0);" onclick="excelDownloadProcess()">엑셀다운로드</a>
</form>
<iframe width=0 height=0 frameborder=0 scrolling=no name="hide_frame" id="hide_frame" style="margin:0"></iframe>
<div id="result"></div>
</body>
</html>
​```

- 엑셀 업로드

MultipartFile에 업로드된 엑셀 파일을 읽어 JSON 형태로 반환

```
	@PostMapping(value = "/uploadExcel")
	@ResponseBody
	public String uploadExcel(MultipartHttpServletRequest request){
		MultipartFile file = null;
		Iterator<String> iterator = request.getFileNames();
		if(iterator.hasNext()) {
			file = request.getFile(iterator.next());
		}

		// 엑셀 헤더 정보 구성
		String[] headerInfo = {"number", "text", "date"};

		// 엑셀 파일을 읽어 데이터 가져오기
		List<HashMap<String, Object>> list = excelUtil.uploadExcel(file, headerInfo);

		return list;
	}
```

- ExcelUtil 클래스 구현

```
	/*
	 * 파일을 읽어 원하는 headerInfo에 따른 값만 추출하여 List 반환
	 */
	public List<HashMap<String, Object>> uploadExcel(MultipartFile excelFile, String[] headerInfo){
		OPCPackage opcPackage = OPCPackage.open(excelFile.getInputStream());
		List<HashMap<String, Object>> list = new ArrayList<HashMap<String, Object>>();
		HashMap<String, Object> data = null;

		// 워크북
		SXSSFWorkbook workbook = null;

		// 행
		SXSSFRow row = null;
	
		// 셀
		SXSSFCell cell = null;

		try {
			// 워크북 생성
			workbook = new SXSSFWorkbook(opcPackage);

			// 첫 번째 시트 불러오기
			sheet = workbook.getSheetAt(0);

			// 행 정보 얻기
			Iterator<Row> rowIterator = sheet.iterator();
			while(rowIterator.hasNext()){
				row = rowIterator.next();
				
				// 셀의 수
				int cells = row.getPhysicalNumberOfCells();
				for(columnIndex = 0; columnIndex < cells; columnIndex++) {
					cell = row.getCell(columnIndex);
					if(cell == null){
						// 셀이 존재하지 않는 경우
						continue;
					} else {
						String value = getCellValueToString(cell);
						data = new HashMap<String, Object>();
						data.put(headerInfo[columnIndex], value);
					}// else
				}// for
				
				// 한 행의 값을 list에 추가
				list.put(data);
			}// while
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(workbook != null) { workbook.close(); }
			} catch (IOException e) {
				e.printStackTrace();
			}// catch
		}// finally

		return list;
	}

	/*
	 * Cell의 값을 String 형태로 반환
	 */
	private String getCellValueToString(Cell cell){
		String value = "";
		switch(cell.getCellType()){
			case SXXFCell.CELL_TYPE_FORMULA:
				value = cell.getCellFormula();
				break;
			case SXXFCell.CELL_TYPE_NUMERIC:
				value = cell.getNumbericCellValue() + "";
				break;
			case SXXFCell.CELL_TYPE_STRING:
				value = cell.getStringCellValue() + "";
				break;
			case SXXFCell.CELL_TYPE_BLANK:
				value = cell.getBooleanCellValue() + "";
				break;
			case SXXFCell.CELL_TYPE_ERROR:
				value = cell.getErrorCellValue() + "";
				break;
		}// switch

		return value;
	}
```   
​

- 엑셀 다운로드

```
	/*
	 * 조회된 데이터를 JSON으로 받아 엑셀 생성
	 */
	@PostMapping(value = "/downloadExcel1")
	@RequestBody
	public void uploadExcel(List<HashMap<String, Object>> list, HttpServletRequest request, HttpServletResponse response){
		// 엑셀 생성
		SXSSWorkbook workbook = ExcelUtil.createExcel(list);
		
		String fileName = ExcelUtil.getFileName("파일명", request);
		String contentType = ExcelUtil.getContentType(request);

		response.setContentType(contentType);
		response.setHeader("Content-Disposition", "attachment; filename=\"" + fileName + "\";");
		response.setHeader("Content-Transfer-Encoding", "binary");

		OutputStream outputStream = null;

		try {
			outputStream = response.getOutputStream();
			workbook.write(outputStream);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(workbook != null) { workbook.close(); }
				if(outputStream != null) { outputStream.close(); }
			} catch (IOException e) {
				e.printStackTrace();
			}// catch
		}// finally
	}

	/*
	 * 데이터 조회 후 엑셀 생성
	 */
	@PostMapping(value = "/downloadExcel2")
	@RequestBody
	public void uploadExcel(HashMap<String, Object> data, HttpServletRequest request, HttpServletResponse response){
		// 검색 조건으로 데이터 조회
		List<HashMap<String, Object>> list = service.selectListData(data);

		// 엑셀 생성
		SXSSWorkbook workbook = ExcelUtil.createExcel(list);
		
		String fileName = ExcelUtil.getFileName("파일명", request);
		String contentType = ExcelUtil.getContentType(request);

		response.setContentType(contentType);
		response.setHeader("Content-Disposition", "attachment; filename=\"" + fileName + "\";");
		response.setHeader("Content-Transfer-Encoding", "binary");

		OutputStream outputStream = null;

		try {
			outputStream = response.getOutputStream();
			workbook.write(outputStream);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(workbook != null) { workbook.close(); }
				if(outputStream != null) { outputStream.close(); }
			} catch (IOException e) {
				e.printStackTrace();
			}// catch
		}// finally
	}
```​

- 엑셀 Util 클래스 구현

```
/*
 * 데이터가 담긴 list를 받아 Excel 생성 후 반환
 */
public SXSSWorkbook createExcel(List<HashMap<String, Object>> list){
	// 워크북
	SXSSFWorkbook workbook = null;

	// 행
	SXSSFRow headerRow = null;
	SXSSFRow row = null;
	
	// 셀
	SXSSFCell headerCell = null;
	SXSSFCell cell = null;

	// 셀 스타일
	CellStyle headerCellStyle = null;
	CellStyle cellStyle = null;

	// 셀 헤더 카운트
	int cellIndex = 0;

	// 행 카운트
	int rowIndex = 0;

	// 엑셀 헤더 정보 구성
	String[] headerInfo = {"number", "text", "date"};

	try {
		// 워크북 생성
		workbook = new SXSSFWorkbook();
		workbook.setCompressTempFiles(true);

		// 시트 생성
		SXSSFSheet sheet = (SXSSFSheet)workbook.createSheet("시트명");
		
		// 헤더 행 생성
		headerRow = sheet.createRow(rowIndex++);

		// 헤더 적용
		for(String head : headerInfo) {
			headerCell = headerRow.createCell(cellIndex++);
			headerCell.setCellValue(info);
			headerCellStyle = new CellStyle();
			headerCell.setCellStyle(applyCellStyle(headerCellStyle, new Color(231, 234, 236)));
		}

		// 바디 적용
		cellIndex = 0; // 셀 카운트 초기화
		for(HashMap<String, Object> rowData : list){
			row = sheet.createRow(rowIndex++);

			for(String headData : headerInfo) {
				String cellData = rowData.get(headData); // HashMap 객체에 Key에 해당하는 값얻기
				cell = row.createCell(cellIndex++); // 셀 생성
				cell.setCellValue(cellData); // 셀에 값 대입
			}
			cellIndex = 0; // 셀 카운트 초기화
		}

	} catch (Exception e) {
		e.printStackTrace();
	} finally {
		try {
			if(workbook != null) { 
				workbook.close(); 
				workbook.dispose();
			}// end if
		} catch (IOException e) {
			e.printStackTrace();
		}// catch
	}// catch

	return workbook;
}
```

```
/*
 * 셀 스타일 정의 후 반환
 */
private CellStyle applyCellStyle(CellStyle cellStyle, Color color) {
	XSSFCellStyle xssfCellStyle = (XSSFCellStyle) cellStyle;

	// 배경색 지정
	xssfCellStyle.setFillForegroundColor(new XSSFColor(color, new DefaultIndexedColorMap()));
	cellStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);

	// 정렬
	cellStyle.setAlignment(HorizontalAlignment.CENTER);
	cellStyle.setVerticalAlignment(VerticalAlignment.CENTER);

	// 테두리
	cellStyle.setBorderLeft(BorderStyle.THIN);
	cellStyle.setBorderTop(BorderStyle.THIN);
	cellStyle.setBorderRight(BorderStyle.THIN);
	cellStyle.setBorderBottom(BorderStyle.THIN);

	return cellStyle;
}
```

```
/*
 * 브라우저에 따른 파일명 인코딩
 */
public static String getFileName(String originalFileName, HttpServletRequest request){
	// 파일명 설정
	Date date = new Date();
	SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyyMMddHHmmss");
	String time = simpleDateFormat.format(date);
	String fileName = "파일명" + "_" + time + ".xlsx";

	// 브라우저 얻기
	String browser = request.getHeader("User-Agent");

	// 브라우저에 따른 파일명 인코딩 설정
	if (browser.indexOf("MSIE") > -1) {
		fileName = URLEncoder.encode(fileName, "UTF-8").replaceAll("\\+", "%20");
	} else if (browser.indexOf("Trident") > -1) {       // IE11
		fileName = URLEncoder.encode(fileName, "UTF-8").replaceAll("\\+", "%20");
	} else if (browser.indexOf("Firefox") > -1) {
		fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1") + "\"";
	} else if (browser.indexOf("Opera") > -1) {
		fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1") + "\"";
	} else if (browser.indexOf("Chrome") > -1) {
		StringBuffer sb = new StringBuffer();
		for (int i = 0; i < fileName.length(); i++) {
			char c = fileName.charAt(i);
			if (c > '~') {
				sb.append(URLEncoder.encode("" + c, "UTF-8"));
			} else {
				sb.append(c);
			}
		}
		fileName = sb.toString();
	} else if (browser.indexOf("Safari") > -1){
		fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1")+ "\"";
	} else {
		fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1")+ "\"";
	}

	return fileName;
}
```

```
/*
 * 브라우저에 따른 ContentType 얻기
 */
public static String getContentType(HttpServletRequest request){
	String browser = request.getHeader("User-Agent");
	String contentType = "application/download;charset=utf-8";
	switch(browser) {
		case "Firefox":
		case "Opera":
			contentType = "application/octet-stream; text/html; charset=UTF-8;";
			break;
		default: // MSIE, Trident, Chrome, ..
			contentType = "application/x-msdownload; text/html; charset=UTF-8;";
			break;
	}

	return contentType;
}
​```

출처 : https://blog.naver.com/hj_kim97/222374962489