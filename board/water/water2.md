---
layout: post
title: "2. SHP파일로 지도위에 폴리곤 표시"
date: 2025-05-03
---

# 1. SHP파일 구하기

<div style="text-align: center;">
  <img src="/사진들/water/SHP1.png" alt="" />
</div>

출처 : [https://egis.me.go.kr/map/map.do]<br>
위 사이트에서 원하는 주제로 SHP파일을 구할 수 있습니다. 예시로 하천구역의 SHP파일을 다운받아 보겠습니다.

<br>

# 2. 원하는 만큼만 자르기

<div style="text-align: center;">
  <img src="/사진들/water/SHP2.png" alt="" />
</div>

QGIS라는 소프트웨어를 사용해서 SHP파일의 원하는 범위만큼(소양강) 편집해야합니다. 전체다 씌우면 길이가 넘 길어서 인식을 못합니다.

<br>

# 3. SHP -> geojson 변환

<div style="text-align: center;">
  <img src="/사진들/water/SHP3.png" alt="" />
</div>

QGIS에서 편집한 레이어를 선택한 후 마우스 우클릭해서 `내보내기`옵션을 선택합니다. 파일 형식을 `geojson`으로 설정하고 여기서 중요한 게 좌표계를 `WGS84`로 맞춰줘야한다는 겁니다. 왜냐하면 네이버지도 API가 `WGS84`를 사용하기때문입니다.

<br>

# 4. 지도에 씌우기

```js
  useEffect(() => {
    const loadMapAndGeoJson = async () => {
      const map = new window.naver.maps.Map("map", {
        center: new window.naver.maps.LatLng(37.926, 127.75),
        zoom: 13,
      });
      mapRef.current = map;

      try {
        const res = await fetch("/data/clip2.geojson");
        const geojson = await res.json();
        setGeoJsonData(geojson);

        for (const feature of geojson.features) {
          const geometry = feature.geometry;

          const drawPolygon = (coords) => {
            const paths = coords.map(
              ([lng, lat]) => new window.naver.maps.LatLng(lat, lng)
            );

            new window.naver.maps.Polygon({
              map,
              paths,
              strokeColor: "#FF0000",
              strokeOpacity: 0.8,
              strokeWeight: 2,
              fillColor: "#880000",
              fillOpacity: 0.4,
            });
          };

          if (geometry.type === "Polygon") {
            drawPolygon(geometry.coordinates[0]);
          } else if (geometry.type === "MultiPolygon") {
            for (const polygon of geometry.coordinates) {
              drawPolygon(polygon[0]);
            }
          }
        }
      } catch (err) {
        console.error("❌ 하천구역 로딩 실패:", err);
      }
    };

    loadMapAndGeoJson();
  }, []);
```

리액트의 `useEffect`를 사용해서 지도를 랜더링하고 그 위에다가 

```js
const res = await fetch("/data/clip2.geojson");
        const geojson = await res.json();
        setGeoJsonData(geojson);

        for (const feature of geojson.features) {
          const geometry = feature.geometry;

          const drawPolygon = (coords) => {
            const paths = coords.map(
              ([lng, lat]) => new window.naver.maps.LatLng(lat, lng)
            );

            new window.naver.maps.Polygon({
              map,
              paths,
              strokeColor: "#FF0000",
              strokeOpacity: 0.8,
              strokeWeight: 2,
              fillColor: "#880000",
              fillOpacity: 0.4,
            });
          };
```

`fetch`로 편집한 `geojson`파일을 불러와서 위도,경도에 따라 반목문을 사용해서 여녹적으로 지도위에 폴리곤으 그려줍니다.
	참고로 `geojson`파일은 이렇게 생겼습니다.
	<div style="text-align: center;">
	  <img src="/사진들/water/SHP4.png" alt="" />
	</div>
