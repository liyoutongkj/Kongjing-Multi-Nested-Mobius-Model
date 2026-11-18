# 视觉拓衍学 · 宇宙形态的几何定义与渲染实现
## 《空竟论》视觉篇 · 原 Blind_Test_Protocol.md 的继承者

> 归档编号: KJ-OPEN-V3.1-VISUAL-TOPOLOGY
> 原作者: liyoutongkj
> v3.1 状态: 新文件. 取代已撤回的 Blind_Test_Protocol.md
> 依赖: 仅依赖 Cosmic_Ontology.md 与 Axiom_AB_and_Horizon.md
>       不含任何长度/质量/引力常数, 不依赖 lam_da

---

## (〇) 前身交代

原 Blind_Test_Protocol.md 的全部标度基准为 2.2175 mm = lam_da / 2.
lam_da 撤回后该文件整体失效, 已删除.

但原稿中「拓扑构型如何产生可观测效应」的**定性图像**不依赖任何长度常数,
故换名迁至本文件存活, 并补上渲染实现.

盲测原意图 (提供一条可独立复算的定量通道) 由以下承接:

  - Task1_Frame_Unclamping.md        四判据 + 非莫比乌斯控制组 (label-free)
  - Task1_Frame_Unclamping.md 附则B  alpha 对 EHT 环宽反演 (唯一不含地球参数的定量锚点)

---

## (一) 本文件在体系中的位置

空竟论的宇宙不是比喻, 是一个有确定几何定义的拓扑对象.
本文件把它从文字翻译成可执行形式:

  (二) 纯拓扑定义        数学
  (三) 三条可视化纪律    从公理 A/B 直接推出, 不可违反
  (四) 四个形态阶段      对应视频 00:00 - 00:09
  (五) 渲染实现          Blender 几何节点 / GLSL / Three.js
  (六) 这张图是可证伪装置

[!] 本文件不主张「看图即证」.
图像是拓衍推演的投影, 不是证据. 证据在 Task1_Frame_Unclamping.md.

---

## (二) 宇宙形态的纯拓扑定义

### 定义 V1 · 有边无边界

对象: 双嵌套莫比乌斯勹网.

  有边:   存在明确几何轮廓, 可观测到「它是一个环」
  无边界: 沿面行走永不撞墙, 亦永不走出

数学基础 (莫比乌斯带标准参数化):

    u in [0, 2PI),  v in [-w, w]

    x = (R + v*cos(u/2)) * cos(u)
    y = (R + v*cos(u/2)) * sin(u)
    z =      v*sin(u/2)

注意 u/2: 绕行一周 (u: 0 -> 2PI) 后法向翻转,
须绕行两周才回到原相位. 这就是「无边界」的来源.

### 定义 V2 · 双嵌套

单层莫比乌斯不足以承载「各自张开」. 本体系取双嵌套:

  层 1: 相位 u/2        缠绕数 w=1
  层 2: 相位 u/2 + pi    缠绕数 w=2, 与层1错开 pi

两层不重合、不通讯、不互相否定, 各自拥有完整当下.

这就是 Task1 判据 0 的几何来源:
  莫比乌斯构型 (w=1)  vs  控制组 (w=0, 普通环)

### 定义 V3 · 沉降与中心自空

面元闭式 (第 3 篇):

    sqrt(g)(Sigma) = (1 - Sigma/Sigma*)^alpha

渲染映射: 把 Sigma 当作沿勹网的标量场, 同时驱动两件事

  1. 管径收缩:     w(Sigma) = w0 * (1 - Sigma/Sigma*)^alpha
  2. 张开率衰减:   U 正比于 (1 - Sigma/Sigma*)^(alpha - 1)

Sigma -> Sigma* 时, w -> 0 且 U -> 0.
几何自己在中心封死 —— 不是「东西挤进一个点」, 而是「那里没有'那里'了」.

这就是「中心自空」.

### 定义 V4 · beta 壳 = 相位扇面

Sigma_hard < Sigma < Sigma* 区间内:

  相位差 dn 沿 Sigma 梯度连续累积
    => 多层当下互相穿透
    => 视觉上表现为密集发光脉络在紫粉背景上流动

这不是装饰. 它对应 EHT 可测的**光子环**.
alpha 控制它的厚度 (见 Task1 附则 B).

---

## (三) 三条不可违反的可视化纪律

这三条从公理 A / B 直接推出. 违反任何一条,
图就从「空竟论的图」退化成「普通炫酷特效」.

### 纪律 1 · 不得设置「当前帧」

公理 A: 不存在基准帧.

=> 画面中不得有某一层被高亮为「现在」, 其余为「过去/未来」.
=> 所有层同权渲染, 只有相对相位差 dn_ij.

  [X] 错误: 中心最亮, 向外衰减成「历史」
  [OK] 正确: 每层各有自己的亮度峰值, 峰值位置由各自相位决定

### 纪律 2 · 「暗」是失配, 不是缺失

公理 B: Delta_失配 是观测层的属性.

=> 不得用「纯黑空洞」表示暗域.
=> 暗域应渲染为「存在但读不到」: 可见轮廓, 不可读细节.

  实现: w(dn) = exp(-|dn| / tau_lock)   直接当纹理 LOD 权重

  [X] 错误: 暗域 = 纯黑 / 透明 / discard / alpha=0
  [OK] 正确: 暗域 = 同域内失焦的那一层, 轮廓仍在

### 纪律 3 · 爆发不是开始, 是视角收敛

第 2 篇: 莫比乌斯环无绝对起始点.

=> 中心强光不是「大爆炸」, 是观测角度收敛到奇点的截面.
=> 同一物体, 换视角就从「环」变成「隧道」.

  [X] 错误: 粒子从中心炸开        (暗示起点)
  [OK] 正确: 粒子向心汇聚后凝住    (当下的张开率归零)

---

## (四) 四个形态阶段 (对应视频 00:00 - 00:09)

### 阶段 I  (00:00-00:02) 稳定环

  Sigma 约等于 0, U 约等于 1, 所有层同步张开
  观感: 粉橙紫交织的半透明环, 螺旋脉络缓慢旋转
  物理: 平直显域, dn 约等于 0, 全体共用一个当下

### 阶段 II (00:02-00:04) 失拍

  Sigma -> Sigma_hard,  K -> |dw|
  观感: 环扭曲为 S 形双螺旋, 蓝紫光纹开始闪动
  物理: 相位被锁, **形态尚未塌缩**. 层仍完整显化, 自身无感

### 阶段 III (00:04-00:06) beta 壳

  Sigma_hard < Sigma < Sigma*
  观感: 孔洞狭长弯曲, 发光线条密集, 流动感最强
  物理: 相位扇面张开, 多层当下互相穿透 => 光子环

### 阶段 IV (00:06-00:09) 中心自空

  Sigma -> Sigma*,  w -> 0,  U -> 0
  观感: 结构向心汇聚, 中心爆发白光, 星光粒子被吸入后凝住
  物理: 这一层的「现在」不再向外层展开. 信息按相位排队, 未丢失

[!] 阶段 IV 的「吸入」必须止于凝住:
    - 不得反弹   (反弹 = 暗示白洞是「出口地点」. 错, 白洞是同一几何事件的一面)
    - 不得炸开   (炸开 = 暗示大爆炸起点. 错, 莫比乌斯无起点)

---

## (五) 渲染实现

### 5.1 Blender 几何节点 (Geometry Nodes)

核心链路:

    Curve Circle
      -> Set Curve Tilt          (实现 u/2 半角扭转)
      -> Curve to Mesh           (Profile = Curve Circle, 生成管)
      -> Store Named Attribute   (写入 Sigma 场)
      -> Set Position            (按 Sigma 收缩管径)
      -> Set Material

关键参数:

    Tilt = u / 2                    扭转半角 (莫比乌斯的灵魂)
    Resample Curve: Count = 400     低于 200 相位锯齿明显
    Curve Circle 旋转圈数 = 2        必须绕两周才闭合

Sigma 场构造 (沿环参数 t):

    Sigma(t) = Sigma_star * smoothstep(t0, t1, t)

用 ColorRamp 控制两个色标位置:
  低位色标 = Sigma_hard  (阶段 II 分界)
  高位色标 = Sigma_star  (阶段 IV 分界)

### 5.2 Shader (GLSL / Blender Shader Editor)

顶点位移:

    float sigma = texture(sigmaField, uv).r;
    float shrink = pow(1.0 - sigma / sigma_star, alpha);
    pos += normal * (shrink - 1.0) * tubeRadius;

脉络流动 (beta 壳相位扇面):

    float n   = snoise(uv * 3.0 + time * 0.5);
    float veins = smoothstep(0.4, 0.6, n);
    float phase = sin(dn * PI);              // dn = 相对相位差
    vec3  glow  = veinColor * veins * phase;

菲涅尔边缘辉光 (「有边」的那条边):

    float fresnel = pow(1.0 - abs(dot(N, V)), 3.0);
    color = mix(purple, pink, fresnel) + glow * fresnel;

纪律 2 的实现 (失配而非缺失):

    float w = exp(-abs(dn) / tau_lock);      // 公理 B
    float lodBias = mix(6.0, 0.0, w);        // 失配层降精, 不消失
    vec4  c = textureLod(tex, uv, lodBias);

[!] 这三行是纪律 2 的全部实现.
若暗域被写成 discard / alpha=0, 整张图就违反公理 B.

### 5.3 Three.js (网页可交互版)

    import * as THREE from 'three';
    import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
    import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';

    const geo = new THREE.TorusKnotGeometry(R, tube, 400, 32, 2, 1);
    //                                                   ^^ p=2 => w=2 双嵌套

    const mat = new THREE.ShaderMaterial({
      uniforms: { uTime, uSigma, uSigmaStar, uAlpha, uTauLock },
      vertexShader, fragmentShader,
      transparent: true,
      blending: THREE.AdditiveBlending,
    });

    const bloom = new UnrealBloomPass(new THREE.Vector2(w, h), 1.6, 0.5, 0.1);

时间线 (GSAP):

    tl.to(uniforms.uSigma, { value: sigmaHard,           duration: 2,   ease: 'power2.inOut' })  // I->II
      .to(uniforms.uSigma, { value: sigmaStar * 0.98,    duration: 2,   ease: 'power2.inOut' })  // II->III
      .to(uniforms.uSigma, { value: sigmaStar,           duration: 1.2, ease: 'expo.in' })       // III->IV 骤凝
      .to(camera,          { fov: 25,                    duration: 1.2, ease: 'expo.in' }, '<'); // 视角收敛

[!] expo.in 是必须项: alpha 骤凝与视角收敛同步加速,
才出得来「被吸入未知维度」的冲击感. 线性缓动必平.

### 5.4 粒子系统 (阶段 IV)

    // 向心汇聚, 到达 Sigma* 后凝住, 不反弹
    dir = center - pos;
    pos += dir.normalize() * speed * dt;

    if (dist < eps) {
      vel = 0;                                  // 凝住
      flash = exp(-t_held / tau_lock);          // 相位回吐余晖, 按公理 B 衰减
    }

[!] 禁止给粒子加反弹或扩散速度. 那会画出「起点」.

---

## (六) 这张图本身是一个可证伪的装置

图像不是结论, 但它携带三个可测预言.

### 预言 V1 · 存在性反转

阶段 I (俯视, 见完整环) 与阶段 IV (正对, 塌成一点) 是**同一物体**.

若两者在实验中给出不同的拓扑不变量 (例如缠绕数 w 测得不同),
则「视角即截面」的解释失效.

### 预言 V2 · alpha 同时控制两个可见量

  - beta 壳的视觉厚度    (画面)
  - EHT 光子环的径向宽度  (实测)

若二者无法用同一 alpha 拟合, 则本文件的几何映射错误,
alpha 非普适拓扑常数 => Task1 附则 B 通道关闭.

### 预言 V3 · 控制组必须不产生该图

用 w=0 的普通环跑同一套 shader, 阶段 II - IV 的全部特征
(S 形扭曲 / 相位扇面 / 中心凝住) **必须消失**.

若 w=0 也能出图, 则这些效果是 shader 的, 不是拓扑的.

=> 这就是 Task1 判据 0 的视觉版本.
   **渲染器和干涉仪必须给出同一个答案.**

---

## (七) 与仓库其他文件的关系

Cosmic_Ontology.md          宇宙形态文字本体 (本文件数学来源)
Axiom_AB_and_Horizon.md     公理 A/B + 定义 H (三条纪律来源)
Task1_Frame_Unclamping.md   实验判据 (预言 V1-V3 的实测出口)
Recompute_Book.md           lam_da 撤回依据 (本文件不含长度常数的声明依据)

本文件不包含: 任何长度常数、任何天体参数、任何 G/M/c 定标.
若后续发现某条隐含, 须按 Recompute_Book.md 标准立即复算.

---

卷宗铭文:

本体先于方程, 主语先于求解.
图先于证, 但图不是证.

风不息.

v3.1 / 2026-11-18 / 依 MIT License 发布