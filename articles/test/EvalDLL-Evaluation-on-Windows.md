The EvalDll library on Windows is provided both as C++ and C# library. A NuGet package is also available at Nuget.org.

## Using the EvalDll library
The EvalDll library enables programmatic model evaluation on CPU (GPU is not supported).  
The usage pattern for this dll is the following:

1. Link the `Cntk.Eval-<VERSION>.lib` import library into the application. Ensure you use the correct file name - see the beginning of this article.
2. Include the evaluation header file "Eval.h"
3. Get an instance of the evaluation engine specific for the model's data type (`float` or `double`).
4. Load the model (or create the network) in the evaluation engine.
5. Evaluate some input against the model and obtain the corresponding output.
6. Dispose the model when done.

For details on the C++ API provided by EvalDll, please refer to the [EvalDll C++ API](./EvalDll-Native-API) page in this wiki.

The [CPPEvalClient](https://github.com/Microsoft/CNTK/tree/master/Examples/Evaluation/CPPEvalClient) program located in the folder [Examples/Evaluation/CPPEvalClient](https://github.com/Microsoft/CNTK/blob/master/Examples/Evaluation/CPPEvalClient) demonstrates the usage of this evaluation interface. Please see the [Eval Examples](./CNTK-Eval-Examples) page for how to build and run examples.

## Using the EvalDll C# library 
CNTK provides a managed (.Net) library wrapper named `Cntk.Eval.Wrapper`. This library wraps the native EvalDll library and exposes a managed interface. This interface provides the same functionality as the native interface, with the addition of some convenience methods.
Alike its native counterpart, this library can only perform evaluations using the CPU (no GPU used). The library is written in CLI/C++ and thus forms the bridge between .Net (e.g. C#) and the native C++ side.

For details regarding the managed API provided by EvalWrapper.DLL, refer to the [EvalDll Managed API](./EvalDll-Managed-API) page.

The usage pattern for the managed wrapper is simple:

    using Microsoft.MSR.CNTK.Extensibility.Managed;
    ...
    try
    {
        using (var model = new IEvaluateModelManagedF())
        {
            // Load model
            model.CreateNetwork(...);
            model.Evaluate(...);
        }
    }
    catch (CNTKException ex)
    {
    ...
    }
    catch (Exception ex)
    {
    ...
    }

There are several [examples](https://github.com/Microsoft/CNTK/blob/master/Examples/Evaluation/CSEvalClient) of performing a programmatic CNTK model evaluation in C# inside the CSEvalClient project. Please see the [Eval Examples](./CNTK-Eval-Examples) page for how to build and run examples.

## NuGet Package
There is currently a NuGet package at Nuget.org (search for CNTK) that provides both the native and managed versions for Debug and Release for the CNTK evaluation libraries (CPU only using MKL). With the NuGet it is possible to simply add the CNTK Eval NuGet to a .Net or Win32 project and call the APIs.
Please refer to the [NuGet Package](./NuGet-Package) page for details on how to get started with CNTK and NuGet.

If you do not want to use NuGet Package, you can add `Cntk.Eval.Wrapper-<VERSION>.dll` as reference to your project. Please make sure in this case that the path to the `Cntk.Eval.Wrapper` dll and its [dependencies](./EvalDll-Evaluation-on-Windows#shipping-eval-v1-library-with-your-windows-application) below are included in the search path of dlls for your application.

## Shipping EvalDll Library with your Windows application
EvalDll requires the Visual C++ Redistributable Package for Visual Studio 2015 to be installed on the system where your application is going to run. 

This [page](./CNTK-Shared-Libraries-Naming-Format) describes how CNTK binary files are named.

If you own application uses EvalDll library you need to distribute these DLLs with your application: 
* `Cntk.Eval-<VERSION>.dll`
* `Cntk.Eval.Wrapper-<VERSION>.dll`
* `Cntk.Math-<VERSION>.dll`
* `libiomp5md.dll`
* `mkl_cntk_p.dll`

All these dlls can be found in the CNTK binary release version, see the [CNTK Releases page](https://github.com/Microsoft/CNTK/releases).