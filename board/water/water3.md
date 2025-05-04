---
layout: post
title: "3. 하천과 오염원들과의 거리계산 & 표시"
date: 2025-05-04
---

# 1. 하천과의 거리계산 공식

```js
  //하천과의 거리계산
  const calculateDistance = (lat1, lon1, lat2, lon2) => {
    const R = 6371e3;
    const φ1 = (lat1 * Math.PI) / 180;
    const φ2 = (lat2 * Math.PI) / 180;
    const Δφ = ((lat2 - lat1) * Math.PI) / 180;
    const Δλ = ((lon2 - lon1) * Math.PI) / 180;

    const a =
      Math.sin(Δφ / 2) ** 2 +
      Math.cos(φ1) * Math.cos(φ2) *
      Math.sin(Δλ / 2) ** 2;

    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

    return R * c;
  };
```

하버사인 공식(Haversine formula)을 사용해서 오염원들과 가까운 하천의 한 지점과의 거리를 구합니다.
하버사인 공식이란 지구 표면상의 두 지점(위도, 경도로 표현됨) 사이의 **대원거리(구면 거리)**를 계산하는 데 사용됩니다. 주로 **지도, GPS, 위치기반 서비스** 등에서 두 지점 간의 실제 거리를 구할 때 사용됩니다.

<br>

# 2. 가까운 하천 찾기

```js
  //가까운 하천이 한 지점 찾기 
  const findClosestPoint = useCallback((markerPos, geojson) => {
    let minDist = Infinity;
    let closestPoint = { lat: null, lng: null };

    geojson.features.forEach((feature) => {
      const geometry = feature.geometry;

      const checkCoords = (coords) => {
        coords.forEach(([lng, lat]) => {
          const dist = calculateDistance(
            markerPos.lat(),
            markerPos.lng(),
            lat,
            lng
          );
          if (dist < minDist) {
            minDist = dist;
            closestPoint = { lat, lng };
          }
        });
      };

      if (geometry.type === "Polygon") {
        checkCoords(geometry.coordinates[0]);
      } else if (geometry.type === "MultiPolygon") {
        geometry.coordinates.forEach((polygon) => checkCoords(polygon[0]));
      }
    });

    return closestPoint;
  }, []);
```

`geojson`파일의 위,경도 모든 지점을 탐색하며 `선택정렬`느낌으로 가장 해당 오염원과 가장 가까운 지점을 찾습니다. 근데 여기서 고민인게 `geojson`파일에 있는 하천들의 위,경도가 상당히 많은데 하나씩 다 완전탐색을 하다보니까 시간복잡도가 너무 오래걸리고 성능도 저하되지 않을까라는 우려가 있습니다. 그래서 이진탐색이나 Hash자료형을 이용해서 성능을 높일 수 있을지 고민중입니다.

<br>

# 3. 화면에 표시

<div style="text-align: center;">
  <img src="/사진들/water/하천거리1.png" alt="" />
</div>

```js
const infoHtml = `
              <div style="padding:8px">
                <strong>${place.bsnm_nm}</strong><br/>
                ${place.bsns_detail_road_addr}<br/>
                ${place.induty_nm}<br/>
                거리: ${Math.round(distance)} 미터
              </div>`;
            infoWindow.setContent(infoHtml);
            infoWindow.open(mapRef.current, marker);
```

`naver.maps`의 API를 이용해서 화면에 표시합니다. 아마, 내부적으로 `useState()`가 선언되어 있는 듯 합니다.
