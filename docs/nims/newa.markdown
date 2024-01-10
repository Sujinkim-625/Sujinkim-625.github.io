---
layout: default
title: Web Application
parent: NIMS 👩‍💻 
# nav_order: 1
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
---
### Shapefile
{: .fs-3 }

[Download the latest SHP file of administrative divisions]("http://www.gisdeveloper.co.kr/?p=2332")   
Shapefiles are a popular file format used in geographic information systems (GIS) for storing the geometric location and attribute information of spatial features.   
Shapefiles store the geometry of spatial features using vector coordinates. This geometry can represent various types of features like points, lines, and polygons (areas).   
[[ESRI Shapefile Technical Description]]("https://www.esri.com/content/dam/esrisites/sitecore-archive/Files/Pdfs/library/whitepapers/pdfs/shapefile.pdf")   
{: .fs-3 }
---
### Shapefile to GeoJSON
{: .fs-3 }

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
---
### GeoJSON to TopoJSON
{: .fs-3 }

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
![d3.js Badge](https://img.shields.io/badge/d3.js-F9A03C?style=flat&logo=d3.js&logoColor=white)
![HTML5 Badge](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=HTML5&logoColor=white)
![JavaScript Badge](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=JavaScript&logoColor=white)
![CSS3 Badge](https://img.shields.io/badge/CSS-1572B6?style=flat&logo=CSS3&logoColor=white)

> 주요 기능   
> 시도 또는 시군구별 해당 날짜의 감염병 관련 데이터를 지도 위에 표시. 지도 위의 색이 진해질수록 확진자(사망자)의 수가 큰 값을 나타냄. 특정 지역 위에 마우스를 올리면 툴팁으로 해당 지역의 정보를 제공.    
{: .fs-3 }
---

### 파일 업로드 기능
{: .fs-3 }

```
    1. csv 파일만 업로드 가능.
    2. 파일 업로드 시, vrc(validateRequiredColumns)를 통해 필수 column이 파일 안에 포함되어있는지 확인. 
    3. 포함되어있지 않다면, 에러 메시지 출력   
    inputs.file({
        label: "파일 업로드",
        accept: ".csv",
        required: true
    })
```
---

### scrubber
{: .fs-3 }

   ```
scrubber를 활용하여 일정시간(setting_delay)마다 날짜가 변경되고, 지도의 색이 해당 날짜의 데이터 값에 따라 변경됨   

Scrubber(formattedDates, {
    delay: setting_delay,
    loop: false,
    autoplay: false,
    format: (d) => d.toLocaleDateString() + "  " + weekOfDay[d.getDay()]
})
   ```
---

### draw the map only once
{: .fs-3 }
   ```
issue > 날짜(데이터)가 달라질 때마다 지도가 새로 그려지는 오류 발생.
fix > 하나의 셀에서 지도 그리기, 지도 색칠하기 모두 작업이 이루어지던 코드를 서로 다른 셀으로 분리하여,   
    파일을 업로드할 때만 지도를 그리고 이후에는 지도 위의 색만 바뀌도록 코드를 변경.    
    데이터 업데이트 후 색이 칠해지는데 걸리는 시간이 줄어듦.  

map.selectAll("path")
    .data(shape.features)
    .join("path")
    .attr("d", path)
    .attr("id", (d) => "map_" + d.id)
    .attr("stroke", "gray")
    .attr("fill", "#f5ebeb")
    .attr("opacity", 0.8)
    .call(tooltip, tooltipDiv);
```
---

### 설정 변경 시, 지도 위의 색 변경 [mutable]({% link docs/nims/mutable.markdown%}#document-d3.js)
{: .fs-3 }
```
issue > 설정 변경(지도를 상하좌우로 이동, map type 변경, zoom, 특정 지역 선택) 시,    
    지도 위의 색이 변경되지 않는 오류 발생. 마우스를 지도 위에 올려야 색이 변경됨.      
fix > 변수를 mutable로 선언.   
    mutable을 사용한 이유: 다른 셀의 값이 변경될 때마다 특정 시점에만 셀을 업데이트, 반응성을 조작하기 위해 사용함. 
    임의의 변수(mutable a)를 선언하고, 다른 mutable 변수들과 하나의 블록 안에서 작동하도록 코드 작성.    
    change color 블록에 a를 선언.   

viewof mutableDataForMapUpdate = {
    mutable a = 0;
    mutable mutable_mrtb = margin_ratio_top_bottom;
    mutable mutable_mrlr = margin_ratio_left_right;
    mutable mutable_mt = mapType;
    mutable mutable_zs = zoomSelect;
    mutable mutable_zoom = zoom;
}
```

```
#change color 블록에 a가 실행되도록 코드 추가
if (a !== "") {
}
```

---
### 툴팁 내용 변경
{: .fs-3 }
```
issue > 툴팁 명이 영어, 옵션(확진자/사망자)에 상관없이 툴팁이 날짜, 지역명, 지역코드, 확진자 수, 사망자 수를 모두 나타냄.
fix> 툴팁 명을 한글로, 옵션에 따라 툴팁에 표시되는 항목을 다르게 설정

const englishToKorean = {
    date: "날짜",
    state: "지역 명",
    positive: "확진자 수",
    death: "사망자 수",
    ratio_positive: "확진자 비율(만 명당)",
    population: "인구 수"
};

// 선택 옵션에 따라 표시되는 항목을 지정하는 객체
const tooltipKeysByOption = {
    positive: ["date", "state", "positive"],
    death: ["date", "state", "death"],
    ratio_positive: ["date", "state", "ratio_positive"]
};
```
---

## download codes from ObservableHQ
### download 
{: .fs-3 }

observableHQ에서 Export를 통해 코드를 다운
{: .fs-3 }

예를 들어. observableHQ에서 작성한 파일이 아래와 같고 Export를 통해 코드를 다운받는다면,
{: .fs-3 }

```javascript
//javascript
viewof amount = Inputs.range([0, 200], {label: "Amount", step: 1})
```
```javascript
//javascript
viewof tipPercent = Inputs.radio([10, 12, 15, 18, 20, 25],
                                 {label: "Tip Percentage", value: 20})
```
```javascript
//javascript
tip = amount * (tipPercent / 100)
```
```markdown
#markdown
A **${tipPercent}%** tip on **$${amount.toFixed(2)}** is **$${tip.toFixed(2)}**,    
for a total of **$${(amount+tip).toFixed(2)}**
```
<br>
### visual studio
{: .fs-3 }

다운로드 받은 코드의 구조는, 아래와 같다.
{: .fs-3 }

```
7d2f01ccb9e8dd3e
    ├─ 7d2f01ccb9e8dd3e@14.js
    ├─ README.md
    ├─ index.html
    ├─ index.js
    ├─ inspector.css
    ├─ package.json
    └─ runtime.js
```
<br>
visual studio에서 압축해제한 파일 전체를 열고 live server를 실행하면,
불필요하다고 생각되는 출력 결과가 함께 보여진다.    
예를 들면,
{: .fs-3 }

```
range 바 아래에, amount = 100
radio button 아래에, tipPercent = 20
```
index.html를 수정하면, 불필요한 출력 결과를 삭제할 수 있다.
{: .fs-3 }
```
# as-is   
const runtime = new Runtime();
const main = runtime.module(define, Inspector.into(document.body));

# be-to
## <body>위에 추가
<section>
  <div id="amount"></div>
  <div id="tipPercent"></div>
  <div id="tip"></div>
</section>

## main을 아래 코드로 수정
const main = runtime.module(define, (name) => {
    if (name === "viewof amount") {
        return new Inspector(document.getElementById("amount"));
    } else if (name === "viewof tipPercent") {
        return new Inspector(document.getElementById("tipPercent"));
    } else if (name === "viewof tip") {
        return new Inspector(document.getElementById("tip"));
    }
});
```
![image1](https://github.com/Sujinkim-625/Sujinkim-625.github.io/blob/main/docs/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-01-10%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.06.19.png?raw=true)

<img src="https://github.com/Sujinkim-625/Sujinkim-625.github.io/blob/main/docs/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-01-10%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.06.19.png?raw=true" width="500" height="300">

---

## electron
{: .fs-4 .fw-500 }


