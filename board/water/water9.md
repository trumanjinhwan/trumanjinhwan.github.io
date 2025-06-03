---
layout: post
title: "9. 오염원 표시와 하천 유역 표시 분리"
date: 2025-06-03
---

# 도입

원래는 `오염원 표시` 체크박스를 누르면 하천 유역 폴리곤이 자동으로 동시에 뜨도로 기능을 구현했습니다. 그러나 프로젝트 멘토님께서 두 개를 분리하는 게 더 좋을 것 같다라고 말씀하셔서 한번 요구대로 구현해보기로 했습니다.

<br>

# 고민

원래 동시에 띄웠던 이유가 지하수의 흐름이 하천 유역(geojson)과 이어지냐 안이어지냐가 오염률 예측에 필요할 거 같아서 그냥 미리 같이 띄어버리고자 했습니다. 근데 두 개를 분리하고자 하니까 고민이 되었습니다. 하천 유역 파일을 로딩을 해놔야 오염원의 지하수 흐름이랑 이어지냐 안이어지냐를 판단할 수 있을 거 같은데, 두 개를 따로 분리해버리면 오염원 표시는 떠도 하천 유역은 안 떠버리니까 두 개를 어떻게 연관지을 수 있을까 고민했습니다.

<br>

# 해결책

해결책을 찾았습니다. `하천 유역` 체크박스가 표시되어 있지 않으면 하천 유역이 투명하게 표시되도록 하고 하천 유역 체크박스가 표시되면 그제서야 색깔로 표시를 하면 되는 것입니다. 즉, 일단 웹사이트에 들어가자 마자 투명색으로 하천 유역으로 자동으로 표시되어 있고 `하천 유역` 체크 박스가 표시가 되면 그 투명을 색깔로 바꾸는 것입니다.

<br>

# 코드 구현

```js
// 처음 로딩 시 GeoJSON 읽고 폴리곤 생성
      try {
        const res = await fetch("/data/clip.geojson");
        const geojson = await res.json();
        setGeoJsonData(geojson);
  
        const newPolygons = geojson.features.map((feature) => {
          const coords =
            feature.geometry.type === "Polygon"
              ? feature.geometry.coordinates[0]
              : feature.geometry.coordinates[0][0];
  
          const path = coords.map(
            ([lng, lat]) => new window.naver.maps.LatLng(lat, lng)
          );
  
          return new window.naver.maps.Polygon({
            map: mapRef.current,
            paths: path,
            strokeColor: "#8000FF",
            strokeOpacity: showWatershed ? 0.8 : 0.0,
            strokeWeight: 2,
            fillColor: "#A56AFF",
            fillOpacity: showWatershed ? 0.3 : 0.0,
          });
        });
  
        setWatershedPolygons(newPolygons);
      } 
```

이 코드에서

```js
fillOpacity: showWatershed ? 0.3 : 0.0,
```

이 삼항 연산자로 `showWatershed`가 true(체크)냐 false(체크X)냐 로 판단해서 하천 유역을 표시하도록 변경해서 문제를 해결했습니다.

<br>

# 실제 화면

<div style="text-align: center;">
  <img src="/사진들/water/분리.png" alt="" />
</div>
