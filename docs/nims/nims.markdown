---
layout: default
title: NIMS 👩‍💻  
nav_order: 2
# has_children: true
# permalink: /docs/nims

---
# National Institute for Mathematical Sciences
{: .no_toc }
---
2023.03-2024.01   
Mathematical modeling for transmission dynamics of infectious diseases   
research intern   
> My works   
1. 감염병 관련 데이터를 시각화한 NIMS Epidemics Data Analysis Web Application 개발.  
   *저작권 등록 완료 (제 C-2023-060091, 국가수리과학연구소 감염병 데이터 시각화 프로그램, 2023.12.14)*
2. uwb 이용 호흡기 감염병 전파 가능 밀접접촉조사 실험 참여 및 데이터 정리 
{: .fs-3 }

---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## From Shapefile to TopoJSON
a transformation process from the Shapefile format to the TopoJSON format, both of which are file formats used in geographic information systems (GIS).
{: .fs-3 }

### Shapefile   
[Download the latest SHP file of administrative divisions]("http://www.gisdeveloper.co.kr/?p=2332")   
Shapefiles are a popular file format used in geographic information systems (GIS) for storing the geometric location and attribute information of spatial features.   
Shapefiles store the geometry of spatial features using vector coordinates. This geometry can represent various types of features like points, lines, and polygons (areas).   
[[ESRI Shapefile Technical Description]]("https://www.esri.com/content/dam/esrisites/sitecore-archive/Files/Pdfs/library/whitepapers/pdfs/shapefile.pdf")   
{: .fs-3 }

### Shapefile to GeoJSON 
- GeoJSON    
GeoJSON is a format for encoding a variety of geographic data structures. 
```
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [125.6, 10.1]
    },
    "properties": {
        "name": "Dinagat Islands"
    }
}
```  
{: .fs-3 }

  - 속성테이블 확인하기 (QGIS)   
    레이어 패널에서 [sig] layer에 마우스 오른쪽 클릭 > '속성 테이블 열기(A)' 클릭
  
  - 다른 파일과 결합하지 않을 때   
    1. [파일명].shp 파일 추가(레이어 패널에 파일 드래그)   
    2. 레이어 패널에서 [sig] layer에 마우스 오른쪽 클릭 > 'Export' > '객체를 다른 이름으로 저장' > 아래의 내용 작성 뒤 '확인' 버튼 클릭   
    *포맷: GeoJSON / 파일 이름: 파일 위치₩[filename].geojson / 좌표계: 프로젝트 좌표계: EPSG:4326 - WGS 84 / 인코딩: UTF-8*   
    3. geoJSON 파일이 생성됨.
   <br>
  - 속성테이블의 추가 정보를 csv에서 가져와 조인하기
    1. [파일명].shp 파일 추가(레이어 패널에 파일 드래그)      
    2. csv 파일 불러오기   
        레이어 > 데이터 원본 관리자 클릭   
        구분자로 분리된 텍스트 클릭 > 파일 선택 > 인코딩 확인(UTF-8) > 도형없음(속성만 있는 테이블) 클릭 > 추가 클릭   
        레이어에 추가된 것을 볼 수 있음    
    3. 속성 결합하기   
        레이어속성> 결합 > + 버튼 클릭     
        벡터 결합 추가하기    
            *결합 레이어: csv 파일명 / 대상 필드를 기준으로 나머지 정보 추가*
        적용 > 확인
        속성 테이블 확인
    4. 레이어 패널에서 sig layer에 마우스 오른쪽 클릭 > Export > 객체를 다른 이름으로 저장
    5. geoJSON 파일이 생성됨
{: .fs-3 }

### GeoJSON to TopoJSON
- TopoJSON   [topojson specification]("https://github.com/topojson/topojson-specification")  
  TopoJSON is an extension of GeoJSON that encodes topology. Rather than representing geometries discretely, geometries in TopoJSON files are stitched together from shared line segments called arcs   
```
{
    "type": "Topology",
    "objects": {
        "dinagatIslands": {
            "type": "GeometryCollection",
            "geometries": [
                {
                    "type": "Point",
                    "properties": {
                        "name": "Dinagat Islands"
                    },
                    "coordinates": [125.6, 10.1]
                }
            ]
        }
    }
}
```
[mapshaper]("https://mapshaper.org/")
1. geoJSON 파일 드래그 인
2. "detect line intersections" 체크 > import 클릭
3. simplify 클릭 > Settings 조절(작을수록 경계가 단순해짐) > "Visvalingam/weighted area" 선택 > apply
4. Export 클릭 > TopoJSON 클릭 > Export 클릭
5. topoJSON 파일이 생성됨
{: .fs-3 }
   
---

## Visualization

---

## download codes from ObservableHQ

---
## eletron
{: .fs-4 .fw-500 }
