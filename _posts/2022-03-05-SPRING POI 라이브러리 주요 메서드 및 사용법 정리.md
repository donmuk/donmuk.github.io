---
layout: post
title: "[SPRING] POI 라이브러리 주요 메서드 및 사용법 정리"
comments: true
description: ""
keywords: ""
tags: [SPRING]]
---

POI란?

공식사이트 : http://poi.apache.org/download.html

아파치 소프트웨어 재단에서 만든 라이브러리로 마이크로소프트 오피스 파일을 자바 언어로 읽고 쓰는 기능을 제공

 - 워드, 엑셀, 파워포인트, .. 등을 지원

 - 최근 오피스 포맷인 Office Open XML File Formats (OOXML, 즉 xml 기반의 *.docx, *xlsx, *.pptx, .. 등) 지원 파일 포맷을 늘려가고 있음

​

주요 클래스

HSSF : 엑셀 97 ~ 2003 버전, 65535 라인까지 사용가능

XSSF : 엑셀 2007 버전 이상, 65535 라인 이상 사용 가능

SXSSF : XSSF의 Streaming Version으로 메모리를 적게 사용, 65535 라인 이상 사용 가능

​

# Maven Dependency

```
<!-- https://mvnrepository.com/artifact/org.apache.poi/poi -->
<!-- 오피스 파일을 자바 언어로 읽고 쓰는 기능을 사용하기 위한 라이브러리 -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.17</version>
</dependency>
<dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi-ooxml</artifactId>
	<version>3.16</version>
</dependency>


<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<!-- Java Object를 JSON으로 변환하거나 JSON을 Java Object로 변환하는데 사용하는 라이브러리 -->
<!-- jackson-databind 라이브러리는 jackson-core 및 jackson-annotation 라이브러리의 의존성을 포함 -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
</dependency>

<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<!-- 파일 업로드를 위한 MultipartFile로 request를 받아주기 위한 라이브러리 -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>
​```

- 주요 메서드

워크북 및 시트 생성

생성 순서 : 워크북 > 시트 > 로우 > 셀

```
// 워크북 생성
SXSSFWorkbook workbook = new SXSSFWorkbook();

// 시트 생성
SXSSFSheet sheet = workbook.createSheet("시트명");

// 시트 열 너비 설정
sheet.setColumnWidth(0, 글자수 * 256);

// row(행) 순서 변수, cell(셀) 순서 변수
int rowCount = 0;
int cellCount = 0;

// row(행) 생성
Row row = sheet.createRow(rowCount++);

// cell(셀) 생성
Cell cell = row.createCell(cellCount++);

// cell(셀) 값 입력
cell.setCellValue('값');
​```


셀 스타일 및 폰트 설정

```
// 셀 Style 정보를 저장할 객체 생성
CellStyle cellStyle = workbook.createCellStyle();

// Style 정보를 Cell에 입력하기
cell.setCellStyle(cellStyle);

// Cell 병합
sheet.addMergedRegion(new CellRangeAddress(시작행, 끝행, 시작열, 끝열);

// 정렬
cellStyle.setAlignment(CellStyle.ALIGN_CENTER); // 가운데 정렬
cellStyle.setAlignment(CellStyle.ALIGN_LIGHT); // 우측 정렬
cellStyle.setAlignment(CellStyle.ALIGN_LEFT); // 좌측 정렬
cellStyle.setVerticalAlignment(CellStyle.VERTICAL_CENTER); // 높이 가운데 정렬
cellStyle.setVerticalAlignment(VerticalAlignment.TOP); // 높이 상단 정렬
﻿
// 테두리 선(좌, 우, 위, 아래)
cellStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);
cellStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);
cellStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);
cellStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN);

// 1. 배경 색상 : Color 사용
Color color = new Color(231, 234, 236); // 그레이색
cellStyle.setFillForegroundColor(new XSSFColor(color, new DefaultIndexedColorMap()));
cellStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);

// 2. 배경 색상 : XSSFCellStyle 사용
XSSFCellStyle xssCellStyle = (XSSFCellStyle) workbook.createCellStyle();
xssCellStyle.setFillForegroundColor(new XSSFColor(new byte[] {(byte)231, (byte)234, (byte)236}, null));
xssCellStyle.setFillPattern(FillPatternType.FINE_DOTS);

// 폰트 설정
Font font = workbook.createFont();
font.setFontName("나눔고딕"); // 글씨체
font.setFontHeight((short)(크기 * 20)); // 사이즈
font.setBoldweight(Font.BOLDWEIGHT_BOLD); // 굵게
font.setBold(true); // 굵게

// 폰트 적용
cellStyle.setFont(font);

// 포맷 설정
cellStyle.setDataFormat(workbook.createDataFormat().getFormat("yyyy-mm-dd")); // 날짜
cellStyle.setDataFormat(workbook.createDataFormat().getFormat("0.00%")); // 퍼센트
cellStyle.setDataFormat(workbook.createDataFormat().getFormat("#,##0")); // 금액
cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("#,##0")); // 금액
cellStyle.setDataFormat(workbook.getCreationHelper().createDataFormat().getFormat("#,##0")); // 금액
파일 출력

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

// 브라우저에 따른 컨텐츠타입 설정
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
response.setContentType("application/x-msdownload; text/html; charset=UTF-8;"); // msie, tRIDE
response.setContentType("application/octet-stream; text/html; charset=UTF-8;");

// response 설정
response.setContentType(contentType);
response.setHeader("Content-Disposition", "attachment; filename=\"" + fileName + "\";");
response.setHeader("Content-Transfer-Encoding", "binary");

// 파일 생성 : OutputStream을 얻어 생성한 엑셀 write
OutputStream outputStream = response.getOutputStream();
workbook.write(outputStream);

// 파일 생성 : FileOutputStream
// FileOutputStream fileOutputStream = new FileOutputStream("파일경로");
// workbook.write(fileOutputStream);
​```


출처 : https://blog.naver.com/hj_kim97/222368341136