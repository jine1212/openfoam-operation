*RAS算例 ?(待完善，)*
---
*openFoam RAS算例*



1/  
算例文件夹
/home/dyfluid/OKS-toUser/OKS2021March/applications/1_incom/2_turbulence/1_stirredTank_SS/cyclic_RANS:


三个文件复制于?

把mesh压缩包解压到constant文件夹

2 /  
修改： constant文件夹中的  momentumTransport  文件 设置为 RAS

3/  
设置0-k ：从p复制三项进去，修改internalfield 0.05

	wall: type  kqRWallFuction;
		 value  uniform 0.005

设置0-epsiol：从k复制三项进去，修改内部场internal 0.03

	wall: type  epsilonWallFuction;
	      value  uniform 0.03    
	outlet:type  epsilonWallFuction;
	      value  uniform 0.03    

设置0-nut：
从k复制三项进去，修改 （湍流粘度从k和ep里面算出来的）

	wall: type calculated;
	outlet: type calculated；   
	        value uniform 1e-5;


ploymesh里面的bounray:
>  outlet改成wall 

fvsolution：

> 改成simple-c  方法上面找， 值均为0.95  ，field 内容注释掉