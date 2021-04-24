*求解器 底层修改 算例3*
---
*openFoam 求解器 底层修改 算例3*


1/  
算例文件夹：
/home/dyfluid/OKS-toUser/OKS2021March/programming/3.Write/8.currentFoam

复制于/home/dyfluid/OpenFOAM/OpenFOAM-8/applications/solvers/incompressible/pisoFoam

2/  
修改 make - files：

 加 _USER_  加 Test

3/  
在fileds.h中加

	Info<< "Reading field alpha\n" << endl;
	volScalarField alpha
	(
	    IOobject
	    (
	        "alpha",
	        runTime.timeName(),
	        mesh,
	        IOobject::MUST_READ,
	        IOobject::AUTO_WRITE
	    ),
	    mesh//0中有alpha边界条件，初值好给，MUST_READ,
	);//在.H场文件中定义标量alpha场，因为c场中有alpha
	
	dimensionedScalar dp(dimLength, 1e-5);//定义在.H场文件中定义标量dp及单位，因为因为c场中有dp
	
	Info<< "Reading field c\n" << endl;
	volScalarField c
	(
	    IOobject
	    (
	        "c",
	        runTime.timeName(),
	        mesh,
	        IOobject::NO_READ,
	        IOobject::AUTO_WRITE
	    ),
	    alpha/(4.0/3.0*M_PI*pow3(dp/2.0)),
	    alpha.boundaryField().types()
	);//因为c才有传输方程，定义源项中的c，c的初值不好给，所以通过alpha算出，并参考alpha边界条件

4/  
在pisoFoam.C中加： 

		dimensionedScalar rhol(dimDensity, 1000);
        dimensionedScalar rhod(dimDensity, 1500);//定义标量rhol和rhod，并给值，因为之后需要
        volScalarField nu = turbulence->nu();
        volScalarField mu = nu*rhol;//定义需要计算的标量nu，mu，因为之后需要
        
        fvScalarMatrix cEqn
        {
           fvm::ddt(c)
         + fvm::div(phi, c)
         - fvc::laplacian(nu, c)
        };
        
        cEqn.solve();//c的传输方程，需要注意单位，注意显隐性
        
        dimensionedScalar g(dimAcceleration, 9.81);//定义标量加速度g，并给值，因为之后需要
        
        volVectorField us = sqr(dp)*(rhod - rhol)*g/18.0/mu*vector(0,-1,0);//定义矢量us，没有传输方程
        
        volVectorField F = -3.0*M_PI*mu*dp*(-us);//定义矢量F，没有传输方程


5/  
在UEqn.H中加源项
+ F*c/rhol

6/  
在lockExchange/constant/transportProperties中第37行
加`transportModel  Newtonian;`

7/  
检查Make/linux64GccDPInt32Opt/variables中
是否为`EXE = $(FOAM_USER_APPBIN)/pisoFoamTest`

8/  
运行后根据提示在fvSchemes中加

	div(phi,c)  Gauss linear;
	div((nuEff*dev2(T(grad(U)))))  Gauss linear;//给格式