*多相流算例*
---
*openFoam 多相流算例  risingBubble*

1/  
算例文件夹：home/dyfluid/OKS-toUser/OKS2021March/applications/4_multiphase/1.VOF/1_risingBubble

2/  
终端 `blockMesh`

3/  
把 turbulenceProperties 名称改为 momentumTransport

4/  
在  momentumTransport 设置 liminar

5/  
修改 0 - P.rgh：
> 除了最后的defaultfaces   empty    其他全部设置为  fixedFluxPressure   （考虑重力的zero）

修改 system - setFieldsDict ：
> volScalarFieldValue alpha.water    1

修改 system - controlDict ：
> writeformat： ascii   （可以看打不开的文件  文件格式的问题）

6/  
终端  ：`setFields`  （划分气泡区域）

7/  
修改 system - fvsolu-  :
> PIMPLE 添加：
> 
> 	pRefCell 0； 
	pRefValue 0；

8/  
终端`interFoam`

ctrl+shift+t

终端`paraFoam` 