---
title: "如何：实现使用发现代理查找服务的客户端应用程序 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 62b41a75-cf40-4c52-a842-a5f1c70e247f
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# 如何：实现使用发现代理查找服务的客户端应用程序
本主题是讨论如何实现发现代理的三个主题中的第三个。在上一个主题[如何：实现向发现代理注册的可检测到的服务](../../../../docs/framework/wcf/feature-details/discoverable-service-that-registers-with-the-discovery-proxy.md)中，您实现了向发现代理注册自身的 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 服务。在本主题中，您将创建一个使用发现代理查找 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 服务的 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端。  
  
### 实现客户端  
  
1.  将一个新控制台应用程序项目添加到名为 `Client` 的 `DiscoveryProxyExample` 解决方案。  
  
2.  添加对下列程序集的引用：  
  
    1.  System.ServiceModel  
  
    2.  System.ServiceModel.Discovery  
  
3.  将位于此主题底部的 GeneratedClient.cs 添加到该项目。  
  
    > [!NOTE]
    >  此文件通常是使用 Svcutil.exe 等工具生成的。此主题中提供的该文件是为了将任务简化。  
  
4.  打开 Program.cs 文件并添加以下方法。此方法使用一个终结点地址，并使用该地址来初始化服务客户端（代理）。  
  
    ```  
    static void InvokeCalculatorService(EndpointAddress endpointAddress)  
            {  
                // Create a client  
                CalculatorServiceClient client = new CalculatorServiceClient(new NetTcpBinding(), endpointAddress);  
                Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress.Uri);  
                Console.WriteLine();  
  
                double value1 = 100.00D;  
                double value2 = 15.99D;  
  
                // Call the Add service operation.  
                double result = client.Add(value1, value2);  
                Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
                // Call the Subtract service operation.  
                result = client.Subtract(value1, value2);  
                Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
                // Call the Multiply service operation.  
                result = client.Multiply(value1, value2);  
                Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
                // Call the Divide service operation.  
                result = client.Divide(value1, value2);  
                Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
                Console.WriteLine();  
  
                // Closing the client gracefully closes the connection and cleans up resources  
                client.Close();  
            }  
  
    ```  
  
5.  将以下代码添加到 `Main` 方法。  
  
    ```  
    public static void Main()  
            {  
                // Create a DiscoveryEndpoint that points to the DiscoveryProxy  
                Uri probeEndpointAddress = new Uri("net.tcp://localhost:8001/Probe");  
                DiscoveryEndpoint discoveryEndpoint = new DiscoveryEndpoint(new NetTcpBinding(), new EndpointAddress(probeEndpointAddress));  
  
                // Create a DiscoveryClient passing in the discovery endpoint  
                DiscoveryClient discoveryClient = new DiscoveryClient(discoveryEndpoint);  
  
                Console.WriteLine("Finding ICalculatorService endpoints using the proxy at {0}", probeEndpointAddress);  
                Console.WriteLine();  
  
                try  
                {  
                    // Search for services that implement ICalculatorService              
                    FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculatorService)));  
  
                    Console.WriteLine("Found {0} ICalculatorService endpoint(s).", findResponse.Endpoints.Count);  
                    Console.WriteLine();  
  
                    // Check to see if endpoints were found, if so then invoke the service.  
                    if (findResponse.Endpoints.Count > 0)  
                    {  
                        InvokeCalculatorService(findResponse.Endpoints[0].Address);  
                    }  
                }  
                catch (TargetInvocationException)  
                {  
                    Console.WriteLine("This client was unable to connect to and query the proxy. Ensure that the proxy is up and running.");  
                }  
  
                Console.WriteLine("Press <ENTER> to exit.");  
                Console.ReadLine();  
            }  
  
    ```  
  
 您已完成客户端应用程序的实现。请继续阅读[如何：测试发现代理](../../../../docs/framework/wcf/feature-details/how-to-test-the-discovery-proxy.md)。  
  
## 示例  
 下面是此主题的完整代码清单。  
  
```  
// GeneratedClient.cs  
//----------------------------------------------------------------  
// Copyright (c) Microsoft Corporation.  All rights reserved.  
//----------------------------------------------------------------  
//------------------------------------------------------------------------------  
// <auto-generated>  
//     This code was generated by a tool.  
//     Runtime Version:2.0.50727.1434  
//  
//     Changes to this file may cause incorrect behavior and will be lost if  
//     the code is regenerated.  
// </auto-generated>  
//------------------------------------------------------------------------------  
  
namespace Microsoft.Samples.Discovery  
{  
    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "4.0.0.0")]  
    [System.ServiceModel.ServiceContractAttribute(Namespace = "http://Microsoft.Samples.Discovery", ConfigurationName = "ICalculatorService")]  
    public interface ICalculatorService  
    {  
  
        [System.ServiceModel.OperationContractAttribute(ProtectionLevel = System.Net.Security.ProtectionLevel.EncryptAndSign, Action = "http://Microsoft.Samples.Discovery/ICalculatorService/Add", ReplyAction = "http://Microsoft.Samples.Discovery/ICalculatorService/AddResponse")]  
        double Add(double n1, double n2);  
  
        [System.ServiceModel.OperationContractAttribute(ProtectionLevel = System.Net.Security.ProtectionLevel.EncryptAndSign, Action = "http://Microsoft.Samples.Discovery/ICalculatorService/Subtract", ReplyAction = "http://Microsoft.Samples.Discovery/ICalculatorService/SubtractResponse")]  
        double Subtract(double n1, double n2);  
  
        [System.ServiceModel.OperationContractAttribute(ProtectionLevel = System.Net.Security.ProtectionLevel.EncryptAndSign, Action = "http://Microsoft.Samples.Discovery/ICalculatorService/Multiply", ReplyAction = "http://Microsoft.Samples.Discovery/ICalculatorService/MultiplyResponse")]  
        double Multiply(double n1, double n2);  
  
        [System.ServiceModel.OperationContractAttribute(ProtectionLevel = System.Net.Security.ProtectionLevel.EncryptAndSign, Action = "http://Microsoft.Samples.Discovery/ICalculatorService/Divide", ReplyAction = "http://Microsoft.Samples.Discovery/ICalculatorService/DivideResponse")]  
        double Divide(double n1, double n2);  
    }  
  
    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "4.0.0.0")]  
    public interface ICalculatorServiceChannel : ICalculatorService, System.ServiceModel.IClientChannel  
    {  
    }  
  
    [System.Diagnostics.DebuggerStepThroughAttribute()]  
    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "4.0.0.0")]  
    public partial class CalculatorServiceClient : System.ServiceModel.ClientBase<ICalculatorService>, ICalculatorService  
    {  
  
        public CalculatorServiceClient()  
        {  
        }  
  
        public CalculatorServiceClient(string endpointConfigurationName) :  
            base(endpointConfigurationName)  
        {  
        }  
  
        public CalculatorServiceClient(string endpointConfigurationName, string remoteAddress) :  
            base(endpointConfigurationName, remoteAddress)  
        {  
        }  
  
        public CalculatorServiceClient(string endpointConfigurationName, System.ServiceModel.EndpointAddress remoteAddress) :  
            base(endpointConfigurationName, remoteAddress)  
        {  
        }  
  
        public CalculatorServiceClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress remoteAddress) :  
            base(binding, remoteAddress)  
        {  
        }  
  
        public double Add(double n1, double n2)  
        {  
            return base.Channel.Add(n1, n2);  
        }  
  
        public double Subtract(double n1, double n2)  
        {  
            return base.Channel.Subtract(n1, n2);  
        }  
  
        public double Multiply(double n1, double n2)  
        {  
            return base.Channel.Multiply(n1, n2);  
        }  
  
        public double Divide(double n1, double n2)  
        {  
            return base.Channel.Divide(n1, n2);  
        }  
    }  
}  
  
```  
  
```  
// Program.cs  
//----------------------------------------------------------------  
// Copyright (c) Microsoft Corporation.  All rights reserved.  
//----------------------------------------------------------------  
  
using System;  
using System.Reflection;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
  
namespace Microsoft.Samples.Discovery  
{  
    class Client  
    {  
        public static void Main()  
        {  
            // Create a DiscoveryEndpoint that points to the DiscoveryProxy  
            Uri probeEndpointAddress = new Uri("net.tcp://localhost:8001/Probe");  
            DiscoveryEndpoint discoveryEndpoint = new DiscoveryEndpoint(new NetTcpBinding(), new EndpointAddress(probeEndpointAddress));  
  
            DiscoveryClient discoveryClient = new DiscoveryClient(discoveryEndpoint);  
  
            Console.WriteLine("Finding ICalculatorService endpoints using the proxy at {0}", probeEndpointAddress);  
            Console.WriteLine();  
  
            try  
            {  
                // Find ICalculatorService endpoints              
                FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculatorService)));  
  
                Console.WriteLine("Found {0} ICalculatorService endpoint(s).", findResponse.Endpoints.Count);  
                Console.WriteLine();  
  
                // Check to see if endpoints were found, if so then invoke the service.  
                if (findResponse.Endpoints.Count > 0)  
                {  
                    InvokeCalculatorService(findResponse.Endpoints[0].Address);  
                }  
            }  
            catch (TargetInvocationException)  
            {  
                Console.WriteLine("This client was unable to connect to and query the proxy. Ensure that the proxy is up and running.");  
            }  
  
            Console.WriteLine("Press <ENTER> to exit.");  
            Console.ReadLine();  
        }  
  
        static void InvokeCalculatorService(EndpointAddress endpointAddress)  
        {  
            // Create a client  
            CalculatorServiceClient client = new CalculatorServiceClient(new NetTcpBinding(), endpointAddress);  
            Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress.Uri);  
            Console.WriteLine();  
  
            double value1 = 100.00D;  
            double value2 = 15.99D;  
  
            // Call the Add service operation.  
            double result = client.Add(value1, value2);  
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Subtract service operation.  
            result = client.Subtract(value1, value2);  
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Multiply service operation.  
            result = client.Multiply(value1, value2);  
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Divide service operation.  
            result = client.Divide(value1, value2);  
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
            Console.WriteLine();  
  
            // Closing the client gracefully closes the connection and cleans up resources  
            client.Close();  
        }  
    }  
}  
```  
  
## 请参阅  
 [WCF Discovery 概述](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)   
 [如何：实现发现代理](../../../../docs/framework/wcf/feature-details/how-to-implement-a-discovery-proxy.md)   
 [如何：实现向发现代理注册的可检测到的服务](../../../../docs/framework/wcf/feature-details/discoverable-service-that-registers-with-the-discovery-proxy.md)