*传热学算例*
---
*openFoam 传热学算例*

1/  
算例文件夹：
home/dyfluid/OKS-toUser/OKS2021March/applications/2_heatTransfer/2_cavity_heat_steadyState

2/  
（1）修改0-U：
前四项都是无滑移  最后empty  
（2）修改0-T.orig：
内部场288， 除了307 288 剩下是zero   最后empty  
（3）修改0-p.rph：
只有wall  一个 fixed  
（4）修改0-p：
前四项calculated 最后一个empty  
（5）修改0-nut：
只有wall ： nutkWall···（壁面函数）  
（6）修改0-k：
壁面函数 
内部系数 设置0.0001  
（7）修改0-es：
壁面函数  内部系数 设置0.001  
（8）修改0-alha t：
 壁面函数


3/  
终端`buoyantS ` (tab补全) 运行

4/  
修改 system - fvsolu ：  
SIMPLE：  添加 momentumPredictor   flase； （ 多相流该命令关闭）

