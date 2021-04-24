*常见知识点*
---
*openFoam 常见知识点*




- **LES - model控制方程：**  
(1)dynamicKEqn  
(2)Smagorinsky




- **RAS - model控制方程：**  
(1)kEpsilon  
(2)SpalartAllmaras





- **-src：版本越老 ，代码看着越简单**
  


- **mu = nu * 密度**  



- **越界，系数不能大于1** 
 


- **方程的表达 ：**

> fvm：：ddt（T）- fvm：：laplacian（DT，T）     即      （αT）/（αt）- ▽·(Dt  ▽T ) = 0 

> fvm：：div（phi：T）   即   要指定 fvscheme 里面的名字      phi = Φ       即    ▽·（ΦT）

> fvc：：grad（p）  即    ▽p

>fvc是显性（已知量），fvm是隐形（未知量）
>
>向量点乘 ：& 


> 标量乘积用*

> ddt（rho，T）  即  （α ρT）/（αt）
> 
> turbulence -divDevTau(U)   即   ▽·τ

> snGrad（） //surfacenormalGrad 面法向梯度   防止震荡


> *mesh.magsf()    //面法向向量的模   防止震荡 
>  
> （rho，T，u） 即   ρTu  
> 
> susp：（）的值>0 , 则，后的项 用隐性   （）的值<0 , 则，后的项 用显性  
> 
> sp： 后面乘以前面（为了解决对角占优的数值计算问题）









