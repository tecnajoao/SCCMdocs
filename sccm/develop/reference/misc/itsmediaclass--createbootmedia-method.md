---
title: "ITsMediaClass::CreateBootMedia Method"
titleSuffix: "Configuration Manager"
ms.date: "09/20/2016"
ms.prod: "configuration-manager"
ms.technology: configmgr-sdk
ms.topic: conceptual
ms.assetid: 5660503c-3bd2-415b-a090-77132e3da898
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.collection: M365-identity-device-management
---
# ITsMediaClass::CreateBootMedia Method
In Configuration Manager, the `CreateBootMedia` method creates task sequence boot media that download policy from a management point and allow a user to run a task sequence for operating system deployment.  

## Syntax  

```  
[IDL]  
HRESULT CreateBootMedia(  
      BSTR MediaType,  
     BSTR DestinationPath,  
     BSTR BootImageID,  
     VARIANT* MediaPassword,  
     VARIANT* CertPath,  
     VARIANT* CertPassword,  
     VARIANT* CertValidStart,  
     VARIANT* CertValidEnd,  
    VARIANT_BOOL Async  
);  
```  

#### Parameters  
 `MediaType`  
 Data type: `BSTR`  

 Qualifiers: [in]  

 The media type. Possible values are:  

|||  
|-|-|  
|CD|A CD. The `DestinationPath` parameter should indicate a path where an ISO file can be created.|  
|UFD|A drive. The `DestinationPath` parameter should indicate the driver letter of a USB Thumb Drive in the format "X:\\".|  
|UFD+FORMAT|A formatted drive. The `DestinationPath` parameter should indicate a drive letter, in the format "X:\\". The drive will be formatted and made bootable. This media type is supported on the Windows Vista operating system only.|  

 `DestinationPath`  
 Data type: `BSTR`  

 Qualifiers: [in]  

 The output file or output drive.  

 `BootImageID`  
 Data type: `BSTR`  

 Qualifiers: [in]  

 The boot image ID from the boot image catalog.  

 `MediaPassword`  
 Data type: `VARIANT`  

 Qualifiers: [in]  

 Pointer to a password to use to encrypt the certificate and other sensitive information. Use null for no password.  

 `CertPath`  
 Data type: `VARIANT`  

 Qualifiers: [in]  

 Pointer to the path to a file in PFX format containing the media certificate. Use null for no certificate.  

 `CertPassword`  
 Data type: `VARIANT`  

 Qualifiers: [in]  

 Pointer to a password for the media certificate.  

 `CertValidStart`  
 Data type: `VARIANT`  

 Qualifiers: [in]  

 Pointer to the time when a newly created certificate starts being valid.  

 `CertValidEnd`  
 Data type: `VARIANT`  

 Qualifiers: [in]  

 Pointer to the time when the newly created certificate stops being valid.  

 `Async`  
 Data type: `VARIANT_BOOL`  

 Qualifiers: [in, defaultvalue(0)]  

 `true` if the creation process is asynchronous. In this case, the caller should poll until the value of [ITsMediaClass::Status Property](../../../develop/reference/misc/itsmediaclass--status-property.md) is 0 and then obtain the return value from [ITsMediaClass::ExitCode Property](../../../develop/reference/misc/itsmediaclass--exitcode-property.md).  

 Set this parameter to `false` for synchronous media creation. See the Return Values section.  

## Return Values  
 An `HRESULT` code. Possible values include, but are not limited to the following value.  

 S_OK  
 The method succeeded.  

 If the `Async` parameter is `false` and the media creation process is synchronous, the method can return the following error codes.  

 CREATETSMEDIA_E_NODEFAULTMP (0x80040001)  
 A default management point has not been assigned for this site.  

 CREATETSMEDIA_E_NOMPCERTS (0x80040002)  
 Certificates for the default management point are not available (internal error).  

 CREATETSMEDIA_E_MISSINGPACKAGE (0x80040003)  
 Package {0} is not available on the specified distribution points.  

 CREATETSMEDIA_E_CERTPASSWORD (0x80040004)  
 Invalid password for media certificate.  

 CREATETSMEDIA_E_PRIVATEKEY (0x80040005)  
 The media certificate does not have an associated private key.  

 CREATETSMEDIA_E_NOCACERT (0x80040006)  
 The certification authority’s certificate has not been set for this site.  

 CREATETSMEDIA_E_NOSITESIGNCERT (0x80040007)  
 The certification authority’s certificate has not been set for this site.  

 CREATETSMEDIA_E_CONNECTIONOPTIONS (0x80040008)  
 Invalid connection options (Configuration Manager SDK only).  

 CREATETSMEDIA_E_CONNECTIONOPTIONNAME (0x80040009)  
 Invalid name for connection option {0} (Configuration Manager SDK only).  

 CREATETSMEDIA_E_UFD_FS_FAT16 (0x8004000A)  
 The specified volume is formatted as FAT16, which is not supported. The file system must be FAT32 or NTFS.  

 CREATETSMEDIA_E_UFD_FS_UNKNOWN (0x8004000B)  
 The specified volume has an unknown file system. The file system must be FAT32 or NTFS.  

 CREATETSMEDIA_E_PACKAGETOOLARGE (0x8004000C)  
 Package {0} is too large to fit on the target media.  

> [!NOTE]
>  Errors that include {0} in the description will have the corresponding value in [ITsMediaClass::ErrorDetail Property](../../../develop/reference/misc/itsmediaclass--errordetail-property.md).  

## Remarks  

> [!CAUTION]
>  `ITsMediaClass::CreateBootMedia` can fail if run concurrently with [ITsMediaClass::CreateCaptureMedia Method](../../../develop/reference/misc/itsmediaclass--createcapturemedia-method.md) or [ITsMediaClass::CreateStandaloneMedia Method](../../../develop/reference/misc/itsmediaclass--createstandalonemedia-method.md), To avoid this situation, your application should call `ITsMediaClass::CreateBootMedia`, `ITsMediaClass::CreateCaptureMedia` and `ITsMediaClass::CreateStandaloneMedia` by using the same.  

## Requirements  
 System Center Configuration Manager.  

## See Also  
 [ITsMediaClass Interface](../../../develop/reference/misc/itsmediaclass-interface.md)   
 [ITsMediaClass::ExitCode Property](../../../develop/reference/misc/itsmediaclass--exitcode-property.md)   
 [ITsMediaClass::Status Property](../../../develop/reference/misc/itsmediaclass--status-property.md)
