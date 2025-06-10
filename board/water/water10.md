---
layout: post
title: "10. 오염률 표시"
date: 2025-06-08
---

# 오염률 예측 공식

<div style="text-align: center;">
  <img src="/사진들/water/오염률.png" alt="" />
</div>

프로젝트 멘토님께서 제시해 주신 이 공식을 바탕으로 지하수 흐름과 융합하여 최종 오염률을 예측하는 기능을 구현했습니다.

# 구현 코드

useState로 값을 받고 공식 적용해서 `ΔC`을 구합니다.

```js
  import React, { useState } from "react";
  import image2 from "./svg/image-2.svg";
  import image from "./svg/image.svg";
  import "./InfoPanel.css";

  export const InfoPanel = ({ setDeltaC }) => {
    const [isVisible, setIsVisible] = useState(true);
    const [temperature, setTemperature] = useState("");
    const [rainfall, setRainfall] = useState("");
    const [humidity, setHumidity] = useState("");
    const [windspeed, setWindspeed] = useState("");
    const [discharge, setDischarge] = useState(""); // 유출량(Q)

    const togglePanel = () => {
      setIsVisible(!isVisible);
    };

    const handleSubmit = () => {
      if (!temperature.trim()) {
        alert("기온을 입력해주세요.");
        document.getElementById("temperature-input").focus();
        return;
      }
      if (!rainfall.trim()) {
        alert("강수량을 입력해주세요.");
        document.getElementById("rainfall-input").focus();
        return;
      }
      if (!humidity.trim()) {
        alert("습도를 입력해주세요.");
        document.getElementById("humidity-input").focus();
        return;
      }
      if (!windspeed.trim()) {
        alert("풍속을 입력해주세요.");
        document.getElementById("windspeed-input").focus();
        return;
      }
      if (!discharge.trim()) {
        alert("유출량을 입력해주세요.");
        document.getElementById("discharge-input").focus();
        return;
      }

      if (isNaN(Number(temperature))) {
        alert("기온을 숫자로 입력하세요.");
        setTemperature("");
        document.getElementById("temperature-input").focus();
        return;
      }
      if (isNaN(Number(rainfall))) {
        alert("강수량을 숫자로 입력하세요.");
        setRainfall("");
        기

      const ΔC = 0.12 * Number(rainfall)
              + 0.06 * Number(temperature)
              - 0.015 * Number(humidity)
              + 0.05 * Number(windspeed)
              + Number(discharge);
              
      console.log("📌 계산된 ΔC:", ΔC); // ✅ 이 줄 추가
      setDeltaC(ΔC); // 💡 App.js의 상태 업데이트
    };

    return (
      <div className="box">
        <div className="panel-wrapper">
          {isVisible && (
            <div className="div weather-form">
              <div className="text-wrapper">날씨 정보 입력</div>
              <img className="image" alt="Image" src={image2} />

              <div className="view-2">
                <div className="overlap-group">
                  <div className="element">
                    <input id="temperature-input" type="text" className="view-3" value={temperature} onChange={(e) => setTemperature(e.target.value)} />
                    <div className="text-wrapper-2">기온</div>
                  </div>
                  <div className="text-wrapper-3">℃</div>
                </div>

                <div className="element-2">
                  <input id="rainfall-input" type="text" className="view-3" value={rainfall} onChange={(e) => setRainfall(e.target.value)} />
                  <div className="text-wrapper-2">강수량</div>
                  <div className="text-wrapper-5">mm</div>
                </div>

                <div className="element-3">
                  <input id="humidity-input" type="text" className="view-3" value={humidity} onChange={(e) => setHumidity(e.target.value)} />
                  <div className="text-wrapper-2">습도</div>
                  <div className="text-wrapper-6">%</div>
                </div>

                <div className="overlap">
                  <div className="element">
                    <input id="windspeed-input" type="text" className="view-3" value={windspeed} onChange={(e) => setWindspeed(e.target.value)} />
                    <div className="text-wrapper-2">풍속</div>
                  </div>
                  <div className="text-wrapper-4">m/s</div>
                </div>

                <div className="element-4">
                  <input id="discharge-input" type="text" className="view-3" value={discharge} onChange={(e) => setDischarge(e.target.value)} />
                  <div className="text-wrapper-2">유출량</div>
                </div>
                <div className="text-wrapper-7">kg</div>

                {/* <div className="text-wrapper-5">mm</div>
                <div className="text-wrapper-6">%</div> */}

                <button className="submit-button" onClick={handleSubmit}>입력하기</button>
              </div>
            </div>
          )}

          <div className="image-wrapper" style={{ left: isVisible ? "258px" : "-30px", top: "30px" }}>
            <button onClick={togglePanel}>
              <img className={`img ${isVisible ? "open" : "closed"}`} alt="Toggle" src={image} />
            </button>
          </div>
        </div>
      </div>
    );
  };

```

<br>

계산된 `ΔC`을 `NaverMapComponent.js`에 넘기기 위해서 모든 컴포넌트의 부모인 `App.js`로 넘깁니다.

```js
import React, { useState } from "react";
import Joyride from "react-joyride";
import NaverMapComponent from "./components/NaverMapComponent";
import { TopPanel } from "./components/TopPanel";
import { InfoPanel } from "./components/InfoPanel";

const App = () => {
  const [run, setRun] = useState(true);
  const [showDEM, setShowDEM] = useState(false);
  const [showWatershed, setShowWatershed] = useState(false);
  const [showPollution, setShowPollution] = useState(false);
  const [showTerrain, setShowTerrain] = useState(false);
  const [deltaC, setDeltaC] = useState(null); // ΔC 상태

  const steps = [
    // {
    //   target: ".top-toggle-buttons",
    //   content: "상단 기능 버튼들입니다. 수질 예측 또는 주제도 기능을 선택하세요.",
    // },
    {
      target: ".top-toggle-buttons", // ✅ 상단 전체 버튼 영역
      content: "DEM과 오염원, 하천 유역, 위성 지도 기능을 선택할 수 있어요.",
      placement: "bottom",
      spotlightPadding: 20, // 옵션: 영역을 더 넓게 강조하고 싶을 때
    },
    {
      target: ".weather-form",
      content: "여기에 날씨 데이터를 입력해주세요.",
    },
    {
      target: "#map",
      content: "지도를 확대하거나 마커를 클릭해 정보를 확인할 수 있습니다.",
    },
  ];

  return (
    <div style={{ position: "relative", width: "100%", height: "100vh" }}>
    {% raw %}
      <Joyride
        steps={steps}
        run={run}
        showSkipButton
        showProgress
        continuous
        styles={{
          options: {
            primaryColor: "#007bff",     // ⬅️ 여기서 Next/Done 버튼 색 변경
            backgroundColor: "#ffffff",
            textColor: "#333333",
            arrowColor: "#ffffff",
            spotlightPadding: 20,
            zIndex: 10000,
          },
        }}
      />
      {% endraw %}

      <NaverMapComponent
        showDEM={showDEM}
        showWatershed={showWatershed}
        showPollution={showPollution}
        showTerrain={showTerrain}
        deltaC={deltaC}
      />

      <div style={{ position: "absolute", top: 20, left: 20, zIndex: 1000 }}>
        <TopPanel
          onToggleDEM={setShowDEM}
          onToggleWatershed={setShowWatershed}
          onTogglePollution={setShowPollution}
          onToggleTerrain={setShowTerrain}
          showDEM={showDEM}
          showWatershed={showWatershed}
          showPollution={showPollution}
          showTerrain={showTerrain}
        />


      </div>

      <div style={{ position: "absolute", top: 200, left: 20, zIndex: 1000 }}>
        <InfoPanel setDeltaC={setDeltaC}/>
      </div>
    </div>
  );
};

export default App;
```

<br>

메인화면인 `NaverMapComponent.js`에서 최종적으로 `ΔC`를 이용해서 오염률을 표시해줍니다.

```js
import React, { useEffect, useRef, useState } from "react";
import * as turf from "@turf/turf";

const NaverMapComponent = ({ showDEM, showWatershed, showPollution, showTerrain, deltaC }) => {
  const [geoJsonData, setGeoJsonData] = useState(null);
  const [watershedPolygons, setWatershedPolygons] = useState([]);
  const [pollutionMarkers, setPollutionMarkers] = useState([]);
  const [demOverlay, setDemOverlay] = useState(null);
  const currentLineRef = useRef(null);
  const mapRef = useRef(null);
  const infoWindowRef = useRef(null);
  // 🔽 컴포넌트 최상단에 추가
  const watershedCirclesRef = useRef([]);

  const MAX_DELTA_C = 25;

  const getPollutionRatePercent = (deltaC) => {
    return Math.min((deltaC / MAX_DELTA_C) * 100, 100);
  };

  const getPolylineColorByPollution = (percent) => {
    if (percent >= 91) return "#FF0000";
    if (percent >= 71) return "#FF8000";
    if (percent >= 51) return "#00AA00";
    if (percent >= 30) return "#00CFFF";
    return "#AAAAAA";
  };

  useEffect(() => {
    console.log("\ud83d\udce1 NaverMapComponent\uc5d0\uc11c \ubc1b\uc740 deltaC:", deltaC);
  }, [deltaC]);

  useEffect(() => {
    const map = new window.naver.maps.Map("map", {
      center: new window.naver.maps.LatLng(37.926, 127.75),
      zoom: 13,
      mapTypeId: window.naver.maps.MapTypeId.NORMAL,
    });
    mapRef.current = map;
  }, []);

  useEffect(() => {
    if (!mapRef.current) return;
    mapRef.current.setMapTypeId(
      showTerrain ? window.naver.maps.MapTypeId.HYBRID : window.naver.maps.MapTypeId.NORMAL
    );
  }, [showTerrain]);

  useEffect(() => {
    const drawWatershed = async () => {
      if (!mapRef.current) return;
      if (watershedPolygons.length > 0) {
        watershedPolygons.forEach((poly) => {
          poly.setOptions({
            fillOpacity: showWatershed ? 0.3 : 0.0,
            strokeOpacity: showWatershed ? 0.8 : 0.0,
          });
        });
        return;
      }

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
      } catch (err) {
        console.error("\u274c clip.geojson \ub85c\ub4dc \uc2e4\ud328:", err);
      }
    };

    drawWatershed();
  }, [showWatershed]);

  useEffect(() => {
    const loadMarkers = async () => {
      if (!mapRef.current) return;

      if (!showPollution) {
        pollutionMarkers.forEach((marker) => marker.setMap(null));
        setPollutionMarkers([]);
        return;
      }

      try {
        const res = await fetch("http://localhost:8080/pollution-sources");
        const data = await res.json();
        const newMarkers = [];

        for (const place of data) {
          const markerPosition = new window.naver.maps.LatLng(
            place.web_bplc_x_katec,
            place.web_bplc_y_katec
          );

          const marker = new window.naver.maps.Marker({
            position: markerPosition,
            map: mapRef.current,
            title: place.bsnm_nm,
          });

          window.naver.maps.Event.addListener(marker, "click", async () => {

            // 📌 ΔC 미입력 시 알림 후 조기 반환
            if (deltaC === null) {
              alert("변인들을 먼저 입력해주십시오.");
              return;
            }

            // ✅ 항상 실행되는 초기화 코드
            if (currentLineRef.current) {
              currentLineRef.current.setMap(null);
              currentLineRef.current = null;
            }
            if (watershedCirclesRef.current) {
              watershedCirclesRef.current.forEach(c => c.setMap(null));
              watershedCirclesRef.current = [];
            }
            if (infoWindowRef.current) {
              infoWindowRef.current.close();
            }

            try {
              const res = await fetch(`/data/flow/flow_path_${place.id}.geojson`);
              const flowGeojson = await res.json();

              const coords = flowGeojson.features[0].geometry.coordinates.map(
                ([lng, lat]) => new window.naver.maps.LatLng(lat, lng)
              );

              let intersects = false;
              let strokeColor = "#AAAAAA";
              let pollutionPercent = 0;

              if (geoJsonData) {
                const polygon = turf.featureCollection(geoJsonData.features);
                const line = turf.lineString(flowGeojson.features[0].geometry.coordinates);
                intersects = turf.booleanIntersects(polygon, line);

                if (intersects && deltaC !== null) {
                  pollutionPercent = getPollutionRatePercent(deltaC);
                  strokeColor = getPolylineColorByPollution(pollutionPercent);
                }

                // 🔽 마커 클릭 이벤트 내부 try 블록 안 intersect 검사 이후에 추가
                if (intersects && deltaC !== null) {
                  pollutionPercent = getPollutionRatePercent(deltaC);
                  strokeColor = getPolylineColorByPollution(pollutionPercent);

                  let invalidGeometryCount = 0;

                  geoJsonData.features.forEach((feature) => {
                    const geometry = feature.geometry;
                    if (!geometry || geometry.coordinates.length === 0) return;

                    let polygons = [];

                    if (geometry.type === "Polygon") {
                      polygons = [geometry.coordinates];
                    } else if (geometry.type === "MultiPolygon") {
                      polygons = geometry.coordinates;
                    } else {
                      return; // Unknown geometry type → skip
                    }

                    polygons.forEach((polyCoords) => {
                      const poly = turf.polygon(polyCoords);
                      const intersections = turf.lineIntersect(poly, line);

                      // ✅ 모든 교차점에 원을 그림
                      intersections.features.forEach((pt) => {
                        const [lng, lat] = pt.geometry.coordinates;
                        const centerLatLng = new window.naver.maps.LatLng(lat, lng);

                        const circle = new window.naver.maps.Circle({
                          map: mapRef.current,
                          center: centerLatLng,
                          radius: 300,
                          strokeColor: strokeColor,
                          strokeOpacity: 1,
                          strokeWeight: 1.5,
                          fillColor: strokeColor,
                          fillOpacity: 0.3,
                        });

                        watershedCirclesRef.current.push(circle);
                      });
                    });
                  });



                  // ✅ 최종 통계 출력
                  if (invalidGeometryCount > 0) {
                    console.warn(`⚠️ 총 ${invalidGeometryCount}개의 유효하지 않은 geometry가 존재합니다.`);
                  }

                }
              }

              const polyline = new window.naver.maps.Polyline({
                map: mapRef.current,
                path: coords,
                strokeColor: strokeColor,
                strokeOpacity: 0.9,
                strokeWeight: 3,
              });

              currentLineRef.current = polyline;

              const infoHtml = `
                <div style="
                  background:white;
                  border:1px solid #888;
                  border-radius:8px;
                  padding:10px;
                  font-size:13px;
                  line-height:1.5;
                  box-shadow: 0 2px 8px rgba(0,0,0,0.25);
                  white-space:nowrap;
                ">
                  <strong>${place.bsnm_nm}</strong><br/>
                  ${place.bsns_detail_road_addr}<br/>
                  ${place.induty_nm}<br/>
                  ${intersects ? `오염률: ${pollutionPercent.toFixed(1)}%` : "<span style='color:gray'>하천 유역과 맞닿지 않음</span>"}
                </div>`;

              const infoWindow = new window.naver.maps.InfoWindow({
                content: infoHtml,
                pixelOffset: new window.naver.maps.Point(0, -60),
                disableAutoPan: true,
              });

              infoWindow.open(mapRef.current, marker);
              infoWindowRef.current = infoWindow;
              mapRef.current.panTo(marker.getPosition());
            } catch (err) {
              alert("흐름 경로를 불러오지 못했습니다.");
              console.error(err);
            }
          });

          newMarkers.push(marker);
        }

        setPollutionMarkers(newMarkers);
      } catch (err) {
        console.error("\u274c 오염원 API 호출 에러:", err);
      }
    };

    loadMarkers();
  }, [showPollution, geoJsonData, deltaC]);

  useEffect(() => {
    if (!mapRef.current) return;

    if (showDEM) {
      const bounds = new window.naver.maps.LatLngBounds(
        new window.naver.maps.LatLng(37.7331240, 127.4980451),
        new window.naver.maps.LatLng(38.0200258, 127.9446376)
      );

      const overlay = new window.naver.maps.GroundOverlay(
        "/images/hillshade.png",
        bounds,
        {
          map: mapRef.current,
          opacity: 0.6,
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

  return (
    <div style={{ position: "absolute", top: 0, left: 0, width: "100%", height: "100%" }}>
      <div id="map" style={{ width: "100%", height: "100%" }} />
    </div>
  );
};

export default NaverMapComponent;

```

# 실제 화면

<div style="text-align: center;">
  <img src="/사진들/water/오염률 화면.png" alt="" />
</div>

# 느낀점

이번 프로젝트를 통해서 딥러닝과 데이터 분석을 구현해볼 수 있는 기회가 생겨서 좋았습니다. 학교 정규 강의로는 잘 다루지 않는 부분들이어서 언젠가 한번 도전해보고 싶은 생각이 있었는데 이번 기회에 해볼 수 있어서 다음에도 기회가 생기면 할 수 있겠다는 자신감이 생겼습니다.
