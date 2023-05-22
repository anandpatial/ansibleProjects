# ansible_role_aix_file_system_expansion
### Developer : Anand Patial
This Play expands an AIX File System (of Independent Nodes or the Cluster Systems) as per the specifications of the MAP Dictionary List\
The "MAP Dictionary List" must be specified in the following format in the Ansible Template Extra Vars Section :

map:

     - host:<fully qualified domain name 1>

       mount_point: <mount point name>

       size_incr: <Increment Value in gigabytes (GB|G|gb|g), megabytes(MB|M|mb|m) or 512 byte blocks>

     - host:<fully qualified domain name 2>

       mount_point: <mount point name>

       size_incr: <Increment Value in gigabytes (GB|G|gb|g), megabytes(MB|M|mb|m) or 512 byte blocks>

     - host: <fully qualified domain name n>

       mount_point: <mount point name>

       size_incr: <Increment Value in gigabytes (GB|G|gb|g), megabytes(MB|M|mb|m) or 512 byte blocks>


## Constraints :
- To avoid *concurrency and locking* issues with the Volume Group "There is One : One mapping of Host and the target Filesystem"
  ```
  This means you can specify Multiple hosts, however, the HOST must not repeat in the dictionary.
  If you need to increase FS size of multiple Filesystems per Host, you have to run multiple iterations of the Automation.

  Hence, for example you have 100 hosts, in a single iteration you could change the size of single filesystem for the 100 hosts 

  ```

## Notes :
```
- The 'Increment value' is needed to be "added"(+increment) to the existing filesystem size. Do not confuse it with the target size

- The Play performs all the checks with respect to the clustermembership and impacts and calculates DecisionPath accordingly.

- The play extracts relevant Cluster System Data and creates a DecisionPath.

- Currently the play supports the File System Expansion (Independent Nodes and Cluster Systems) where FS is under OS control or HACMP control.

- The Play performs all the Validation checks with respect to the Filesystem existence, the identification of the Volume and the Volume Group and the feasibility of the expansion and takes the Mirroring and MAX LPs in account as well.

- NodesParametersParser enables the users to define a "Nodes-Parameters Mapping" in the Ansible tower that enables choice of different filesystem and sizes for each inventory node.

- The Play makes use of roles. And these roles are reusable in other Plays (provided the Global Variable list is maintained) with tweaks as appropriate.
```

## Technical Details :
### Program Control :
```
- group_vars/target_servers.yml
- FSExpansion.yml
```
### Roles :
```
- NodesParametersParser/tasks/main.yml
- ValidateFS_DetectVG/tasks/main.yml
- GatherClusterData/tasks/main.yml
- DecisionPath/tasks/main.yml
- WorkOnIncrementValue/tasks/main.yml
- FindAvlbSpace/tasks/main.yml
- DecideGoNoGo/tasks/main.yml
- ExecFSIncrease/tasks/main.yml
```