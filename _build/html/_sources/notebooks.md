# 建立几何参数

## 体积几何

创建二维体积几何结构：

`````{tab-set} 
````{tab-item} Python

```{code-block} python
vol_geom = astra.create_vol_geom(n_rows_and_cols)#创建行列数相等的2d体积结构
vol_geom = astra.create_vol_geom((n_rows, n_cols))
vol_geom = astra.create_vol_geom(n_rows, n_cols)
vol_geom = astra.create_vol_geom(n_rows, n_cols, min_x, max_x, min_y, max_y)#任意指定体积的范围

vol_geom = astra.create_vol_geom([n_rows, n_cols, n_slices])#创建3d体积结构
vol_geom = astra.create_vol_geom(n_rows, n_cols, n_slices)
vol_geom = astra.create_vol_geom(n_rows, n_cols, n_slices, min_x, max_x, min_y, max_y, min_z, max_z)#任意指定体积的范围（注意行沿Y轴方向，列沿X轴方向）
```
````
````{tab-item} Matlab
```{code-block}matlab
vol_geom = astra_create_vol_geom(n_rows_and_cols);%创建行列数相等的2d体积结构
vol_geom = astra_create_vol_geom([n_rows n_cols]);
vol_geom = astra_create_vol_geom(n_rows, n_cols);
vol_geom = astra_create_vol_geom(n_rows, n_cols, min_x, max_x, min_y, max_y);%任意指定体积的范围

vol_geom = astra_create_vol_geom([n_rows, n_cols, n_slices]);%创建3d体积结构
vol_geom = astra_create_vol_geom(n_rows, n_cols, n_slices);
vol_geom = astra_create_vol_geom(n_rows, n_cols, n_slices, min_x, max_x, min_y, max_y, min_z, max_z);%任意指定体积的范围，行是沿 Y 轴定向的，列是沿 X 轴定向的，slice是沿 Z 轴定向的
```
````
`````

```{warning}
注意：在GPU代码中使用时，体积必须以原点为中心且像素必须为正方形。并非所有函数都会显式检查这些要求，不遵守可能导致不可预测的结果。
```
## 标准轨迹投影几何

### 平行束(parallel)

创建二维平行束投影几何结构：

`````{tab-set} 
````{tab-item} Python
```{code-block} python
proj_geom = astra.create_proj_geom('parallel', det_spacing, det_count, angles)

```
````
````{tab-item} Matlab
```{code-block}matlab
proj_geom = astra_create_proj_geom('parallel', det_spacing, det_count, angles);
```
````
`````
参数说明：
- det_spacing: 相邻探测器像素中心之间的距离
- det_count: 单个投影中的探测器像素数量
- angles: 投影角度（弧度）

### 扇形束(fanflat)

创建二维扇形束投影几何结构：

`````{tab-set} 
````{tab-item} Python
```{code-block} python
proj_geom = astra.create_proj_geom('fanflat', det_spacing, det_count, angles, source_origin, origin_det)

```
````
````{tab-item} Matlab
```{code-block}matlab
proj_geom = astra_create_proj_geom('fanflat', det_spacing, det_count, angles, source_origin, origin_det);
```
````
`````
参数说明：
- det_count: 单个投影中的探测器数量
- vectors: 包含实际几何结构的矩阵。每行对应一个投影，包含6个元素：(srcX, srcY, dX, dY, uX, uY)
  - src: 射线源
  - d: 探测器中心
  - u: 探测器像素0到1的向量

以下是将"fanflat"类型投影几何转换为6元素行的示例：

```{tabbed} Python
```python
# 射线源
vectors[i,0] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginSource']
vectors[i,1] = -numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginSource']

# 探测器中心
vectors[i,2] = -numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginDetector']
vectors[i,3] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginDetector']

# 探测器像素0到1的向量
vectors[i,4] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorWidth']
vectors[i,5] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorWidth']


```{tabbed} Matlab
```matlab
% 射线源
vectors(i,1) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginSource;
vectors(i,2) = -cos(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginSource;

% 探测器中心
vectors(i,3) = -sin(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginDetector;
vectors(i,4) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginDetector;

% 探测器像素0到1的向量
vectors(i,5) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorWidth;
vectors(i,6) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorWidth;


工具箱中也提供了转换函数：

```{tabbed} Python
```python
proj_geom_vec = astra.geom_2vec(proj_geom)

```{tabbed} Matlab
```matlab
proj_geom_vec = astra_geom_2vec(proj_geom);