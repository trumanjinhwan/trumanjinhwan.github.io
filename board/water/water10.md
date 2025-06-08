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
  const [discharge, setDischarge] = useState("");

  const togglePanel = () => {
    setIsVisible(!isVisible);
  };

  const handleSubmit = () => {
    if (!temperature.trim() || isNaN(Number(temperature))) {
      alert("기온을 숫자로 입력해주세요.");
      setTemperature("");
      document.getElementById("temperature-input").focus();
      return;
    }
    if (!rainfall.trim() || isNaN(Number(rainfall))) {
      alert("강수량을 숫자로 입력해주세요.");
      setRainfall("");
      document.getElementById("rainfall-input").focus();
      return;
    }
    if (!humidity.trim() || isNaN(Number(humidity))) {
      alert("습도를 숫자로 입력해주세요.");
      setHumidity("");
      document.getElementById("humidity-input").focus();
      return;
    }
    if (!windspeed.trim() || isNaN(Number(windspeed))) {
      alert("풍속을 숫자로 입력해주세요.");
      setWindspeed("");
      document.getElementById("windspeed-input").focus();
      return;
    }
    if (!discharge.trim() || isNaN(Number(discharge))) {
      alert("유출량을 숫자로 입력해주세요.");
      setDischarge("");
      document.getElementById("discharge-input").focus();
      return;
    }

    const ΔC = 0.12 * Number(rainfall)
              + 0.06 * Number(temperature)
              - 0.015 * Number(humidity)
              + 0.05 * Number(windspeed)
              + Number(discharge);
    
    console.log("📌 계산된 ΔC:", ΔC);
    setDeltaC(ΔC);
  };

  return (
    <div className="box">
      <div className="panel-wrapper">
        {isVisible && (
          <div className="div weather-form">
            <div className="text-wrapper">날씨 정보 입력</div>
            <img className="image" alt="Image" src={image2} />
            <!-- 입력 필드 생략 -->
            <button className="submit-button" onClick={handleSubmit}>입력하기</button>
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
<Joyride
  steps={steps}
  run={run}
  showSkipButton
  showProgress
  continuous
  styles={{
    options: {
      primaryColor: "#007bff",
      backgroundColor: "#ffffff",
      textColor: "#333333",
      arrowColor: "#ffffff",
      spotlightPadding: 20,
      zIndex: 10000,
    },
  }}
/>
```

---

# 실제 화면

<div style="text-align: center;">
  <img src="/사진들/water/오염률 화면.png" alt="" />
</div>
