---
layout: post
title: "[SPRING] POI 라이브러리 Excel 파일 읽기"
comments: true
description: ""
keywords: ""
tags: [DB]
---

# POI 라이브러리 Excel 파일 읽기



- Excel 파일 읽기

1. FileInputStream() 객체를 사용하여 읽어들이기

```
// Excel 2007 이상인 경우
FileInputStream fis = new FileInputStream(new File("파일 경로"));
XSSFWorkbook workbook = new XSSFWorkbook(fis);

// Excel 97 ~ 2003 버전인 경우
FileInputStream fis = new FileInputStream(new File("파일 경로"));
HSSFWorkbook workbook = new HSSFWorkbook(fis);
​```

2. File 객체를 사용하여 읽어들이기

```
// Excel 97 ~ 2007 모두 가능
Workbook workbook = WorkbookFactory.create(new File("파일 경로"));
​```

3. OPCPackage 객체를 사용하여 읽어들이기, 2007 버전 이상인 경우만 해당

```
OPCPackage opcPackage = OPCPackage.open(new File("파일 경로"));
XSSFWorkbook workbook = new XSSFWorkbook(opcPackage);
​```

- Excel 내용 얻기

```
OPCPackage opcPackage = OPCPackage.open(new File("파일 경로"));
XSSFWorkbook workbook = new XSSFWorkbook(opcPackage);

// 시트의 수
int sheets = workbook.getNumberOfSheets();
for(int i = 0; i < sheets; i++){
	Sheet sheet = workbook.getSheetAt(i);
	System.out.println("Sheet Name : " + sheet.getSheetName() + "\n");

	/* 1. row 얻기 : getPhysicalNumberOfRows();
	int rows = sheet.getPhysicalNumberOfRows();
	for(int r = 0; r < rows; r++){
		..
	}
	*/

	// 2. row 얻기 : iterator();
	Iterator<Row> rowIterator = sheet.iterator();
	int rowIndex = 0;
	while(rowIterator.hasNext()) {
		Row row = rowIterator.next();
		System.out.println(rowIndex + "행 :\n");

		/* 1. cell 얻기 : getPhysicalNumberOfCells();
		int cells = row.getPhysicalNumberOfCells();
		for(int c = 0; c < cells; c++){
			..
		}
		*/

		// 2. cell 얻기 : cellIterator();
		Iterator<Cell> cellIterator = row.cellIterator();
		while(cellIterator.hasNext()) {
			Cell cell = cellIterator.next();

			switch(cell.getCellType()) {
				case Cell.CELL_TYPE_BOOLEAN:
					System.out.print(cell.getBooleanCellValue() + "\t");
					break;
				case Cell.CELL_TYPE_NUMERIC:
					System.out.print(cell.getNumericCellValue() + "\t");
					break;
				case Cell.CELL_TYPE_STRING:
					System.out.print(cell.getStringCellValue() + "\t");
					break;
				case Cell.CELL_TYPE_FORMULA:
					System.out.print(cell.getCellFormulaCellValue() + "\t");
					break;
			}// switch
		}// while
		System.out.println("");
	}// while
}// for
​```


출처 : https://blog.naver.com/hj_kim97/222374990512