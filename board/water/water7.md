---
layout: post
title: "7. DEM 고도 값을 이용한 지하수 흐름 유추"
date: 2025-05-13
---

# 고도 값 -> 지하수 흐름 이론

<div style="text-align: center;">
  <img src="/사진들/water/지하수1.png" alt="" />
</div>
<div style="text-align: center;">
  <img src="/사진들/water/지하수2.png" alt="" />
</div>

위의 내용들을 가지고 DEM의 고도 값을 바탕으로 지하수의 흐름을 예측할 수 있습니다. 2개의 이미지를 간단히 설명하자면 지표면 위의 높낮이와 지표면 아래의 지하수 흐름은 거의 유사하다라는 내용입니다.

<br>

# 유추 알고리즘 코드(Numpy, Rasterio)

```python
import os
import pymysql
import geojson
import rasterio
import numpy as np
from math import sqrt
from rasterio.transform import rowcol

# ✅ DEM 경로 설정
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
TIF_PATH = os.path.join(BASE_DIR, "data", "prac.tif")

# ✅ prac.tif 로드
with rasterio.open(TIF_PATH) as src:
    elevation = src.read(1)
    transform = src.transform
    crs = src.crs
    nodata = src.nodata

# ✅ NoData 처리 및 gradient 계산
elevation = np.ma.masked_equal(elevation, nodata)
dy, dx = np.gradient(elevation)
K = 0.0001  # 유압 전도도
vx = -K * dx
vy = -K * dy
STEP_SIZE = 1.0

# ✅ 지하수 흐름 경로 추적 함수
def trace_flow_path(start_lat, start_lng, max_steps=200):
    try:
        row, col = rowcol(transform, start_lng, start_lat)
    except:
        return []

    path = []

    for _ in range(max_steps):
        if (row < 0 or row >= elevation.shape[0] or
            col < 0 or col >= elevation.shape[1]):
            break

        lng, lat = transform * (col, row)
        path.append((lat, lng))

        vx_val = vx[row, col]
        vy_val = vy[row, col]
        mag = sqrt(vx_val**2 + vy_val**2)
        if mag == 0 or np.ma.is_masked(mag):
            break

        row -= int(round((vy_val / mag) * STEP_SIZE))
        col += int(round((vx_val / mag) * STEP_SIZE))

    return path

# ✅ DB 연결 정보
conn = pymysql.connect(
    host="www.lifeslike.org",
    user="water",
    password="water1111",
    database="weather_db",
    charset="utf8mb4",
    use_unicode=True
)
cursor = conn.cursor()

# ✅ 오염원 좌표 가져오기
cursor.execute("""
    SELECT id, latitude, longitude 
    FROM pollution_sources
    WHERE latitude IS NOT NULL AND longitude IS NOT NULL
""")
rows = cursor.fetchall()

# ✅ 결과 저장 경로: ../my-weather-map/public/flow/
output_dir = os.path.join(BASE_DIR, "..", "my-weather-map", "public", "data", "flow")
os.makedirs(output_dir, exist_ok=True)

count = 0

for id, lat, lng in rows:
    path = trace_flow_path(lat, lng)
    if not path or len(path) < 2:
        continue

    feature = geojson.Feature(
        geometry=geojson.LineString([(lng_, lat_) for lat_, lng_ in path]),
        properties={"id": id}
    )
    geojson_obj = geojson.FeatureCollection([feature])

    file_path = os.path.join(output_dir, f"flow_path_{id}.geojson")
    with open(file_path, "w", encoding="utf-8") as f:
        geojson.dump(geojson_obj, f, ensure_ascii=False, indent=2)

    count += 1

print(f"✅ 지하수 흐름 GeoJSON {count}개 생성 완료")

cursor.close()
conn.close()

```

오염원의 위,경도에서 지하수 흐름을 유추하는 코드입니다.

```python
# ✅ 지하수 흐름 경로 추적 함수
def trace_flow_path(start_lat, start_lng, max_steps=200):
    try:
        row, col = rowcol(transform, start_lng, start_lat)
    except:
        return []

    path = []

    for _ in range(max_steps):
        if (row < 0 or row >= elevation.shape[0] or
            col < 0 or col >= elevation.shape[1]):
            break

        lng, lat = transform * (col, row)
        path.append((lat, lng))

        vx_val = vx[row, col]
        vy_val = vy[row, col]
        mag = sqrt(vx_val**2 + vy_val**2)
        if mag == 0 or np.ma.is_masked(mag):
            break

        row -= int(round((vy_val / mag) * STEP_SIZE))
        col += int(round((vx_val / mag) * STEP_SIZE))

    return path

```

이 코드가 핵심인데, 위, 경도를 인자로 받아서 DEM고도 값을 기준으로 기울기를 계산하고 그 기울기에 따라 흐르는 경로를 (위도, 경도) 형식의 튜플들을 path 배열에 넣습니다.

<br>

# 생성된 geojson 형식의 파일들

<div style="text-align: center;">
  <img src="/사진들/water/지하수3.png" alt="" />
</div>

위의 이미지처럼 각각 오염원들마다의 geojson파일이 생기고 그것들을 이용해서 맵에다가 폴리곤형태로 표시해주면 됩니다.

<br>

# 완성본

<div style="text-align: center;">
  <img src="/사진들/water/지하수4.png" alt="" />
</div>
