### 材质
> 材质结合几何体，构成Mesh对象。Mesh对象才可被添加到场景中。材质就像物体的皮肤，决定了物体看起来是否像金属、透明与否、或者是否显示为线框。

#### 1.材质种类(16种)

| 名称 | 描述 |
|---|---|
| MeshBasicMaterial(网格基础材质) | 基础材质，用于给几何体赋予一种简单的颜色 |
| MeshDepthMaterial(网格深度材质) |  |
| MeshNormalMaterial(网格法向材质) |  |
| MeshFaceMaterial(网格面材质) |  |
| MeshLambertMaterial(网格Lambert材质) |  |
| MeshPhongMaterial(网格Phong式材质) |  |
| MeshPhysicalMaterial(网格物理材质) |  |
| MeshStandardMaterial(网格标准材质) |  |
| MeshToonMaterial(网格Toon材质) |  |
| PointsMaterial(点集材质) |  |
| RawShaderMaterial(原始着色器材质) |  |
| ShaderMaterial(着色器材质) |  |
| ShadowMaterial(阴影材质) |  |
| LineBasicMaterial(直线基础材质) |  |
| LineDashMaterial(虚线材质) |  |
| SpriteMaterial(精灵材质) |  |

#### 2.材质的共性

Three.js提供了一个材质基类THREE.Material，它列出了所有的共有属性。将这些共有属性可以分为三类：
- 基础属性： 控制物体的不透明度、是否可见以及如何被引用
- 融合属性： 决定物体如何与背景融合
- 高级属性： 控制底层WebGL上下文对象渲染物体的方式，大多数情况下不需要使用这些属性

##### 2.1基础属性
- id
材质实例的唯一标识。第一个材质的值从0开始，每新加一个材质，这个值就增加1
- uuid
这是自动生成的唯一ID，只读，在内部使用，
- name
可以通过这个属性赋予材质名称，用于调试的目的。默认为空，可以不唯一。
- opacity
定义物体的透明度。与transparent属性一起使用，该属性的赋值范围为0.0到1.0。默认为1.0。transparent设置为true的时候，opacity才起作用，如果transparent为false，则opacity不生效。
- transparent
如果该值设置为true，Three.js会使用指定的不透明度渲染物体。如果设置为false，这个物体就不透明。如果使用alpha(透明度)通道的纹理，该属性应该设置为true
- overdraw
绘制时的三角形展开量。当你使用THREE.CanvasRender时，多边形会被渲染得稍微大一点。渲染器渲染的物体有间隙时，可以调节这个属性，0.5倾向于在浏览器中给出良好的效果。默认为0。
- visible
定义该材质是否可见。设置为false，那么在场景中就看不到该物体。默认为true
- side
定义几何体的哪一面应用材质。默认值为THREE.FrontSize(前面)。也可以将其设置为THREE.BackSide(后面)，这样可以将材质应用到物体的后面，或者也可以将它设置为THREE.DoubleSize(双侧),将材质应用到物体的内外两侧。

##### 2.2融合属性
- blending
决定物体上的材质如何与背景融合。一般的融合模式是THREE.NormalBlending，在这种模式下只显示材质的上层。除了标准融合模式之外，还有THREE.NoBlending、THREE.AdditiveBlending、THREE.SubtractiveBlending、THREE.MultiplyBlendig、THREE.CustomBlending
- blendSrc
融合源。除了标准融合模式之外，可以通过设置blendsrc、blenddst和blendequation来创建自定义的融合模式。这属性定义了该物体如何与背景相融合，默认值为THREE.SrcAlphaFactor，即使用alpha(透明度)通道进行融合。设置改属性值时，blending的值必须为THREE.CustomBlending。
- blendDst
融合目标。该属性定义了融合时如何使用背景，默认值为THREE.OneMinusSrcAlphaFactor,其含义是目标也使用源的alpha通道进行融合，只是使用的值是1（源的alpha通道值）。设置改属性值时，blending的值必须为THREE.CustomBlending。
- blendEquation
融合公式。定义了如何使用blendSrc和blendDst的值。默认值为使它们相加（AddEquation）。通过使用这三个属性，可以创建自定义的融合模式。

最后一组属性大多在内部使用，用来控制WebGL渲染场景时的细节。

##### 2.3高级属性
- depthTest
使用这个属性可以打开或者关闭GL_DEPTH_TEST参数。此参数控制是否使用像素深度来计算新像素的值。通常情况下不必修改这个属性。
- depthWrite
这个属性可以用来决定这个材质是否影响WebGL的深度缓存。如果你将一个物体用作二维贴图，应该将这个属性设置为false。但是，通常不应该修改这个属性。
- polygonOffset\polygonOffsetFactor和polygonOffsetUnits
可以控制WebGL的POLYGON_OFFSET_FILL特性。
- alphaTest
可以给这个属性指定一个值（0 ~ 1）,如果某个像素的alpha值小于该值，那么该像素不会显示出来。可以用这个熟悉感移除一些与透明度相关的毛边。默认是0。


#### 3.创建材质

##### 3.1创建方式

- 传参

```javascript
  var material = new THREE.MeshBasicMaterial({
    color: 0xff0000, name: 'basic-material', opacity: 0.5, transparent: true, ...
  })
```

- 实例

```javascript
var material = new THREE.MeshBasicMaterial();
material.color = new THREE.Color(0xff0000);
material.name = 'basic-material';
material.opacity = 0.5;
material.transparent = true;
```

两种方式比较：
最好的方式是在创建材质对象时使用构造方法传入参数。
两种方式中参数使用相同的格式，唯一例外的是color属性，第一种方式传入十六进制值，Three.js会自己创建一个THREE.Color对象，第二种方式，必须创建一个THREE.Color对象。

##### 3.2具体材质

###### 3.2.1 THREE.MeshBasicMaterial

这是一种非常简单的材质，这种材质不考虑场景中光照的影响。使用这种材质的网格会被渲染成简单的平面多边形，或者显示几何体的线框。

特别的属性
- color 
材质的颜色
- wireframe
是否将材质渲染成线框
- wireframeLinewidth
定义线框中线的宽度。windows，线框始终为1。WebGLRenderer模式下，无效。
- wireframeLinecap
线框模式下，顶点间线段的端点如何显示，可选值有"butt(平)"、"round(圆)"、"square(方)"，默认值为round。WebGLRenderer对象不支持该属性
- wireframeLinejoin
线段的连接点如何显示。可选值有"round(圆角)"、"bevel(斜角)"、"miter(尖角)"。同样，WebGLRenderer对象不支持该属性。
- vertexColors
顶点颜色。默认值为THREE.NoColors。如果设置为THREE.VertexColors,渲染器会采用THREE.Geometry对象的colors属性的值。
- fog
指定当前材质是否受全局雾化效果设置的影响。

###### 3.2.2 THREE.MeshDepthMaterial

使用这种材质的物体，其外观不是由光照或某个材质属性决定的，而是由物体到摄像机的距离决定的。可以将这种材质与其他材质结合使用，创建出逐渐消失的效果。
属性：

- wireframe
是否显示线框
- wireframeLinewidth
线框线的宽度

##### 3.2.3 THREE.MeshNormalMaterial
使用该材质的物体，每一面的颜色是由该面向外指的法向量计算得到的。


##### 3.2.4 THREE.MeshLambertMaterial
这种材质可以用来创建暗淡的并不光亮的表面。对场景中的光源产生反应。

- emissive
定义该材质反射的颜色。默认值为黑色。一种纯粹的，不受其他光照影响的颜色。


##### 3.2.5 THREE.MeshPhongMaterial
创建一种光亮的材质。
属性与THREE.MeshLambertMaterial基本差不多。
- specular
指定该材质的光亮程度以及高光部分的颜色。如果设置成与color属性相同的颜色，将会得到一个更加类似金属的材质。如果设置成灰色，材质变得更像塑料。
-shininess
该属性指定镜面高光部分的亮度。默认30。




