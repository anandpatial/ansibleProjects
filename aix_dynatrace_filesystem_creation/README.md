# Dynatrace Filesystems Creation on the AIX Nodes
### Developer : Anand Patial
This Automation creates the AIX Filesystems as per the specifications of the MAP Dictionary List\
The "MAP Dictionary List" must be specified in the following format in the Ansible Template Extra Vars Section :

```markdown
map:
  - host: <Host Fully Qualified Domain Name> 
    
    mount_point: <FS Mount Full Path>
    
    lv_name: <Logical Volume Name>
    
    lv_copies: <Logical Volume Copies>
    
    vg: <Volume Group Name>
    
    size: <Size Value in gigabytes (GB|G|gb|g), megabytes(MB|M|mb|m)


  - host: <Host Fully Qualified Domain Name> 
    
    mount_point: <FS Mount Full Path>
    
    lv_name: <Logical Volume Name>
    
    lv_copies: <Logical Volume Copies>
    
    vg: <Volume Group Name>
    
    size: <Size Value in gigabytes (GB|G|gb|g), megabytes(MB|M|mb|m)

```
## Constraints :

- To avoid *concurrency and locking* issues with the Volume Group "There is One : One mapping of Host and the target Filesystem"
  ```
  This means you can specify Multiple hosts, however, the HOST must not repeat in the dictionary.
  If you need to create multiple Filesystems per Host, you have to run multiple iterations of the Automation

  Hence to achieve the creation of /opt/dynatrace and /var/lib/dynatrace on 100 hosts you would need to run TWO iterations of the Automation

  In first Iteration you will create /opt/dynatrace for all the 100 hosts
  In second Iteration you will create /var/lib/dynatrace for all the 100 hosts 

  ```
- Unix Engineering Services Checks Enforced
  ```
  The Volume Group allowed : "rootvg"
  The Allowed Logical Volume | Filesystems are 
    lv_dynatrc | /opt/dynatrace
    lv_dtlib | /var/lib/dynatrace
  ```

## Technical Details :
### Program Control :
```
- group_vars/target_servers.yml
- FSCreation.yml
```
### Roles :
```
- NodesParametersParser/tasks/main.yml
- UESDynatraceCheck/tasks/main.yml
- CheckObjectsPreExistence/tasks/main.yml
- ValidateVG/tasks/main.yml
- WorkOnIncrementValue/tasks/main.yml
- FindAvlbSpace/tasks/main.yml
- DecideGoNoGo/tasks/main.yml
- CreateLV/tasks/main.yml
- CreateFS/tasks/main.yml
```
  

  



