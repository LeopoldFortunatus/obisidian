```
grep rpc test/sandbox/vmcontroller/proto/v1/api.proto|awk '{print "\| " $2 " \| \| \| \| \| \|" }'
```
# Vmcontroller methods

## V1

| Method                                      | admin | editor | viewer | devops | all | comment |
| ------------------------------------------- | ----- | ------ | ------ | ------ | --- | ------- |
| CreateJobNode                               |       |        |        | +      |     |         |
| DeleteJobNode                               |       |        |        | +      |     |         |
| GetJobNode                                  |       |        |        | +      |     |         |
| ListJobsNode                                |       |        |        | +      |     |         |
| CreateJobVirtualMachine                     |       |        |        | +      |     |         |
| DeleteJobVirtualMachine                     |       |        |        |        |     |         |
| GetJobVirtualMachine                        |       |        |        |        |     |         |
| ListJobsVirtualMachine                      |       |        |        |        |     |         |
| CreateJobVmImage                            |       |        |        |        |     |         |
| DeleteJobVmImage                            |       |        |        |        |     |         |
| GetJobVmImage                               |       |        |        |        |     |         |
| ListJobsVmImage                             |       |        |        |        |     |         |
| CreateJobVmMigrate                          |       |        |        |        |     |         |
| DeleteJobVmMigrate                          |       |        |        |        |     |         |
| GetJobVmMigrate                             |       |        |        |        |     |         |
| ListJobsVmMigrate                           |       |        |        |        |     |         |
| CreateJobVirtualMachineResize               |       |        |        |        |     |         |
| DeleteJobVirtualMachineResize               |       |        |        |        |     |         |
| GetJobVirtualMachineResize                  |       |        |        |        |     |         |
| ListJobsVirtualMachineResize                |       |        |        |        |     |         |
| CreateJobBackup                             |       |        |        |        |     |         |
| DeleteJobBackup                             |       |        |        |        |     |         |
| GetJobBackup                                |       |        |        |        |     |         |
| ListJobsBackup                              |       |        |        |        |     |         |
| CreateJobCreateVolume                       |       |        |        |        |     |         |
| CreateJobDeleteVolume                       |       |        |        |        |     |         |
| CreateJobResizeVolume                       |       |        |        |        |     |         |
| DeleteJobVolume                             |       |        |        |        |     |         |
| GetJobVolume                                |       |        |        |        |     |         |
| ListJobsVolume                              |       |        |        |        |     |         |
| CreateNamespace                             |       |        |        |        |     |         |
| DeleteNamespace                             |       |        |        |        |     |         |
| GetNamespace                                |       |        |        |        |     |         |
| ListNamespaces                              |       |        |        |        |     |         |
| CreateNetspace                              |       |        |        |        |     |         |
| DeleteNetspace                              |       |        |        |        |     |         |
| GetNetspace                                 |       |        |        |        |     |         |
| ListNetspaces                               |       |        |        |        |     |         |
| CreateNetwork                               |       |        |        |        |     |         |
| DeleteNetwork                               |       |        |        |        |     |         |
| GetNetwork                                  |       |        |        |        |     |         |
| ListNetworks                                |       |        |        |        |     |         |
| CreateNode                                  |       |        |        |        |     |         |
| DeleteNode                                  |       |        |        |        |     |         |
| GetNode                                     |       |        |        |        |     |         |
| ListNodes                                   |       |        |        |        |     |         |
| GetStorage                                  |       |        |        |        |     |         |
| ListStorages                                |       |        |        |        |     |         |
| CreateVirtualMachine                        |       |        |        |        |     |         |
| DeleteVirtualMachine                        |       |        |        |        |     |         |
| GetVirtualMachine                           |       |        |        |        |     |         |
| ListVirtualMachines                         |       |        |        |        |     |         |
| SetExternalNetworkAddress                   |       |        |        |        |     |         |
| UnsetExternalNetworkAddress                 |       |        |        |        |     |         |
| ListExternalNetworkAddresses                |       |        |        |        |     |         |
| GetVolume                                   |       |        |        |        |     |         |
| ListVolumes                                 |       |        |        |        |     |         |
| GetVirtualMachineVNC                        |       |        |        |        |     |         |
| AddSSHKeys                                  |       |        |        |        |     |         |
| AddSSHUser                                  |       |        |        |        |     |         |
| GetSSHUser                                  |       |        |        |        |     |         |
| RemoveSSHKeys                               |       |        |        |        |     |         |
| ResetRootPassword(ResetRootPasswordRequest) |       |        |        |        |     |         |
| GetAuditNodeById                            |       |        |        |        |     |         |
| ListAuditNode                               |       |        |        |        |     |         |
| GetAuditNamespaceById                       |       |        |        |        |     |         |
| ListAuditNamespace                          |       |        |        |        |     |         |
| GetAuditVmById                              |       |        |        |        |     |         |
| ListAuditVm                                 |       |        |        |        |     |         |
| GetDatabase                                 |       |        |        |        |     |         |



# Gateway methods

