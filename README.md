# oms-agent-troubleshooting

Deployed PCF 2.7.0
- Ops Manager v2.7.0
- PAS (Small Runtime) v2.7.0

Stemcell Used by PAS
- [ubuntu-xenial 456.27](https://bosh.io/stemcells/bosh-warden-boshlite-ubuntu-xenial-go_agent#v456.27)

No additional hardening was done on the virtual machine. 

Deployed OMS Linux Agent Bosh Release hosted on (github)
- [Src, Deployment Instructions & Releases](https://github.com/Azure/oms-agent-for-linux-boshrelease)
- [OMS Linux Agent v1.9.0](https://github.com/Azure/oms-agent-for-linux-boshrelease/releases/tag/v1.9.0-0)

# Error Message
See file `pre-start.stdout.log`
```
VERBOSE from InstallModule.py: Installing resource MSFT_nxFileInventoryResource
VERBOSE from InstallModule.py: Updated permissions of file: /opt/omi/lib/libMSFT_nxFileInventoryResource_root-oms.so to 0644
VERBOSE from InstallModule.py: Updated permissions of file: /etc/opt/omi/conf/omiregister/root-oms/MSFT_nxFileInventoryResource.reg to 0644
VERBOSE from OMS_MetaConfigHelper.py: OMS config path being read: /etc/opt/microsoft/omsagent/1eb9ef69-7c4e-480d-979b-143fec05011d/conf/omsadmin.confFailed to open the dsc log file /var/opt/microsoft/omsconfig/omsconfig.log

VERBOSE from OMS_MetaConfigHelper.py: Output from: /opt/microsoft/omsconfig/Scripts/SetDscLocalConfigurationManager.py -configurationmof /etc/opt/omi/conf/omsconfig/generated_meta_config.mof: instance of SendConfigurationApply
{
    ReturnValue=0
}


ERROR from OMS_MetaConfigHelper.py: Error on running command: /opt/microsoft/omsconfig/Scripts/SetDscLocalConfigurationManager.py -configurationmof /etc/opt/omi/conf/omsconfig/generated_meta_config.mof Error Message: Traceback (most recent call last):
  File "/opt/microsoft/omsconfig/Scripts/SetDscLocalConfigurationManager.py", line 99, in <module>
    main(argv)
  File "/opt/microsoft/omsconfig/Scripts/SetDscLocalConfigurationManager.py", line 33, in main
    operationStatusUtility.write_failure_to_status_file_no_log(operation, 'Python exception raised from SetDscLocalConfigurationManager.py: ' + formattedExceptionMessage)
  File "/opt/microsoft/omsconfig/Scripts/OperationStatusUtility.py", line 265, in write_failure_to_status_file_no_log
    write_to_status_file(operation, 'false', errorMessage)
  File "/opt/microsoft/omsconfig/Scripts/OperationStatusUtility.py", line 220, in write_to_status_file
    mkdir(statusFolderPath)
OSError: [Errno 13] Permission denied: '/var/opt/microsoft/omsconfig/status' 
```

# Hypothesis
OMS agent when installed does not get the proper user permisions on stemcell
- [reference1](https://github.com/Azure/oms-agent-for-linux-boshrelease/blob/master/jobs/omsagent/templates/install.erb#L96)
- [reference2](https://github.com/Azure/oms-agent-for-linux-boshrelease/blob/master/jobs/omsagent/templates/install.erb#L118)

[General Information on BOSH Releases](https://bosh.io/docs/create-release/) and how to create / update them
