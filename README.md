<div align="center">
    
 # 🌎 Maps 🗺️ 

![typescript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)

</div>

Development of an application that generates two random points in Brazil and simulates a flight between these points, taking into account the curvature of the Earth. It uses an IBGE API to control the coordinates and identify the state of Brazil through which the plane is passing.

> [Visit the website here!](https://front-maps.vercel.app/)

## ⚙️ How to run

1. Clone the repository and start the submodules.

        git clone https://github.com/felipe-sant/Maps.git
        cd Maps
        git submodule update --init --recursive

2. Download the dependencies in both repositories.

       cd Back
       npm install
       cd ..
    
       cd Front
       npm install

3. Run both repositories with the command:

       npm start

## 📘 Contents covered

- <a href="#calculation-of-geodesic-lines">Calculation of geodesic lines;</a>
- <a href="#implementation-of-external-apis">Implementation of external APIs.</a>
- <a href="#map-rendering">Map rendering;</a>

<hr>

<h3 id="calculation-of-geodesic-lines">Calculation of geodesic lines</h3>

#### 1. Conversion to Radians
   
The latitude and longitude angles, given in degrees, are converted to radians:

    lat₁ = startPoint.lat × π/180
    lon₁ = startPoint.lon × π/180

    lat₂ = endPoint.lat × π/180
    lon₂ = endPoint.lon × π/180

#### 2. Calculation of the Angle Between Points (Δσ)

The central angle (Δσ) between the two points on the sphere is computed using the dot product in 3D space:

    Δσ = arccos(sin(lat₁) ⋅ sin(lat₂) + cos(lat₁) ⋅ cos(lat₂) ⋅ cos(lon₂ - lon₁))
 
if Δσ = 0, the points are identical, and the function directly returns the starting point.

#### 3. Interpolation Coefficients (𝑎 and 𝑏)

The coefficients 𝑎 and 𝑏 determine the contribution of each point in the interpolation, using trigonometric functions:

    𝑎 = sin((1 - 𝑡) ⋅ Δσ) / sin(Δσ)

    𝑏 = sin(𝑡 ⋅ Δσ) / sin(Δσ)

Here:

- 𝑡 is the interpolation parameter (0 corresponds to the starting point, and 1 corresponds to the ending point).

- sin(Δ𝜎) normalizes the coefficients to ensure correct interpolation along the spherical arc.

#### 4. Interpolation in 3D Space

The points are treated as 3D vectors projected onto the sphere, with coordinates:

    𝑥 = 𝑎 ⋅ cos(lat₁) ⋅ cos(lon₁) + 𝑏 ⋅ cos(lat₂) ⋅ cos(lon₂)

    𝑦 = 𝑎 ⋅ cos(lat₁) ⋅ sin(lon₁) + 𝑏 ⋅ cos(lat₂) ⋅ sin(lon₂)

    𝑧 = 𝑎 ⋅ sin(lat₁) + 𝑏 ⋅ sin(lat₂)

#### 5. Conversion Back to Latitude and Longitude

The interpolated coordinates (𝑥, 𝑦, 𝑧) are converted back to latitude and longitude:

    lat = arctan2(𝑧, √(𝑥² + 𝑦²))
    
    lon = arctan2(𝑦, 𝑥)

Finally, the values are converted from radians to degrees:

    lat = lat × 180/π
    lon = lon × 180/π

#### Mathematical Summary of the Function

The interpolation calculates the intermediate point 𝑡 along the spherical arc between two points on the Earth's surface, with:

1. Δσ determining the angular distance between the points.
2. 𝑎 and 𝑏 weighting the vectors of the start and end points.
3. The resulting 3D vectors being converted back into geographic coordinates.

This enables smooth movement of a point between two locations while maintaining the geodesic trajectory.

<hr>

<h3 id="implementation-of-external-apis">Implementation of external APIs</h3>

In this project, I used an external API from **IBGE** to implement geographic meshes. The API was utilized to identify geographic coordinates and retrieve information about the state or municipality a specific coordinate references. This integration ensured accurate mapping of coordinates to administrative regions.

<div align="center">
    
<img src="https://servicodados.ibge.gov.br/api/v3/malhas/paises/BR?intrarregiao=UF" height="256px" width="256px" />
    
</div>

<hr>

<h3 id="map-rendering">Map rendering</h3>

One of the key topics covered in this project was map rendering, using the **Leaflet** library. With Leaflet, it was possible not only to render interactive maps but also to manage the coordinates of points on the map, ensuring precise control over the location and interaction of geographic elements. This approach enabled the creation of dynamic and functional visualizations for efficiently exploring spatial data.

<hr>

<div align="center">
    developed by <a href="https://github.com/felipe-sant?tab=followers">@felipe-sant</a>
</div>
