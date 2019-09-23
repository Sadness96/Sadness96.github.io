---
title: 电脑硬件信息帮助类
date: 2017-06-06 15:29:18
tags: [c#,helper,windows]
categories: C#.Net
---
<img src="https://raw.githubusercontent.com/Sadness96/sadness96.github.io/master/images/blog/csharp-DevFramework/%E6%B3%A8%E5%86%8C%E5%B7%A5%E5%85%B7.png"/>

### 获取电脑硬件基础信息，用于开发时作为唯一标示或注册激活使用
<!-- more -->
#### 帮助类及说明
[PCInformationHelper](https://github.com/Sadness96/Sadness/blob/master/Code/Helper/Utils.Helper/PCInformation/PCInformationHelper.cs) 获取的信息有：网卡MAC地址、CPU-ID、硬盘序列号、内存序列号、主板序列号、BIOS序列号、显卡信息。可拼接加密生成唯一序列号，可用于软件激活使用或作为其他唯一标识。
``` CSharp
/// <summary>
/// 读取网卡MAC地址
/// </summary>
/// <returns>成功返回网卡MAC地址,失败返回NULL</returns>
public static List<string> MAC()
{
    try
    {
        List<string> listMAC = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("Win32_NetworkAdapterConfiguration");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            if ((bool)mo["IPEnabled"])
            {
                strMac = mo["MacAddress"].ToString();
                listMAC.Add(strMac);
            }
        }
        return listMAC;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}

/// <summary>
/// 读取CPU-ID
/// </summary>
/// <returns>成功返回CPU-ID,失败返回NULL</returns>
public static List<string> CPU()
{
    try
    {
        List<string> listCPU = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("Win32_Processor");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            //Manufacturer = 处理器制造商
            //Name = 处理器名字
            //Processorid = CPU-ID
            strMac = mo["Processorid"].ToString();
            listCPU.Add(strMac);
        }
        return listCPU;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}

/// <summary>
/// 读取硬盘序列号
/// </summary>
/// <returns>成功返回硬盘序列号,失败返回NULL</returns>
public static List<string> DESK()
{
    try
    {
        List<string> listDESK = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("win32_DiskDrive");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            //Model = 硬盘信息
            //SerialNumber = 硬盘序列号
            strMac = mo["SerialNumber"].ToString();
            listDESK.Add(strMac);
        }
        return listDESK;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}

/// <summary>
/// 读取内存序列号
/// </summary>
/// <returns>成功返回内存序列号,失败返回NULL</returns>
public static List<string> Memory()
{
    try
    {
        List<string> listMemory = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("Win32_PhysicalMemory");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            //Manufacturer = 内存生产商
            //SerialNumber = 序列号
            strMac = mo["SerialNumber"].ToString();
            listMemory.Add(strMac);
        }
        return listMemory;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}

/// <summary>
/// 读取主板序列号
/// </summary>
/// <returns>成功返回主板序列号,失败返回NULL</returns>
public static List<string> Motherboard()
{
    try
    {
        List<string> listMotherboard = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("WIN32_BaseBoard");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            //Manufacturer = 主板制造商
            //Product = 主板型号
            //SerialNumber = 序列号
            strMac = mo["SerialNumber"].ToString();
            listMotherboard.Add(strMac);
        }
        return listMotherboard;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}

/// <summary>
/// 读取BIOS序列号
/// </summary>
/// <returns>成功返回BIOS序列号,失败返回NULL</returns>
public static List<string> BIOS()
{
    try
    {
        List<string> listBIOS = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("Win32_BIOS");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            //Manufacturer = BIOS制造商名称
            //SerialNumber = BIOS序列号
            //ReleaseDate = 出厂日期
            //Version = 版本号
            strMac = mo["SerialNumber"].ToString();
            listBIOS.Add(strMac);
        }
        return listBIOS;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}

/// <summary>
/// 读取显卡信息
/// </summary>
/// <returns>成功返回显卡信息,失败返回NULL</returns>
public static List<string> Video()
{
    try
    {
        List<string> listVideo = new List<string>();
        string strMac = "";
        ManagementClass mc = new ManagementClass("Win32_VideoController");
        ManagementObjectCollection moc = mc.GetInstances();
        foreach (ManagementObject mo in moc)
        {
            //Name = 显卡信息
            //DriverVersion = 驱动程序版本
            strMac = mo["Name"].ToString();
            listVideo.Add(strMac);
        }
        return listVideo;
    }
    catch (Exception ex)
    {
        TXTHelper.Logs(ex.ToString());
        return null;
    }
}
```