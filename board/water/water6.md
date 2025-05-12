---
layout: post
title: "6. 지도 위에 DEM(수치표고모델) 씌우기"
date: 2025-05-12
---


# DEM 파일 구하기

<div style="text-align: center;">
  <img src="/사진들/water/DEM1.png" alt="" />
</div>

출처 : [https://map.ngii.go.kr/ms/map/NlipMap.do?tabGb=total]

위의 사이트에서 원하는 지역의 DEM파일들을 가져올 수 있습니다.

<br>

# QGIS로 파일 병합

<div style="text-align: center;">
  <img src="/사진들/water/DEM2.png" alt="" />
</div>
<div style="text-align: center;">
  <img src="/사진들/water/DEM3.png" alt="" />
</div>

얻어온 DEM파일(.img 확장자)들을 하나의 레이어로 합쳐줍니다. <br>
QGIS를 실행하고 `레스터 -> 기타사항 -> 병합` 순으로 간 다음 원하느 파일들을 선택해서 `실행`버튼을 눌러줍니다.

<br>

# QGIS로 음영기복도(hillshade)로 변환

<div style="text-align: center;">
  <img src="/사진들/water/DEM4.png" alt="" />
</div>

병합한 레이어 누르시고 `마우스 우클릭 -> 속성 -> 심볼 -> 음영기복도`로 변환 및 추출해줍니다.

<br>

# tif -> png 변환

<div style="text-align: center;">
  <img src="/사진들/water/DEM5.png" alt="" />
</div>

음영기복도로 변환한 레이어 누르시고 `마우스 우클릭 -> 내보내기 -> 다른 이름으로 저장` 한 다음에 형식은 GeoTIFF로 해서 `확인`버튼 눌러줍니다.

<br>

<div style="text-align: center;">
  <img src="/사진들/water/DEM6.png" alt="" />
</div>
출처 : [https://cloudconvert.com/tif-to-png]

위의 사이트로 들어가서 png로 변환해줍니다.

<br>

# 지도 위에 표시

<div style="text-align: center;">
  <img src="/사진들/water/DEM7.png" alt="" />
</div>

```js

//지도 위에 DEM 관련 png 띄우는 부분만 발췌한 것

const [demOverlay, setDemOverlay] = useState(null);// DEM관련 png를 담음
const mapRef = useRef(null); //지도 API를 담음

useEffect(() => {
    if (!mapRef.current) return;

    if (showDEM) {
      const bounds = new window.naver.maps.LatLngBounds(
        new window.naver.maps.LatLng(37.7331240, 127.4980451), // SW
        new window.naver.maps.LatLng(38.0200258, 127.9446376)  // NE
      );

      const overlay = new window.naver.maps.GroundOverlay(
        "/images/hillshade.png",
        bounds,
        {
          map: mapRef.current,
          opacity: 0.6, // 투명도
        }
      );
      setDemOverlay(overlay);
    } else {
      if (demOverlay) {
        demOverlay.setMap(null);
        setDemOverlay(null);
      }
    }
  }, [showDEM]);
```

useState와 useRef를 이용해서 지도 위에 png를 띄워줍니다.
