![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述           | 作者     |
| :--------- | ----------- | ------------------ | -------- |
| 2023-12-20 | 1.0         | 特性测试报告初稿完 | lihongji |

 关键词： 

openGauss、timescaleDB

摘要：

本文档主要介绍timescaledb插件适配openGauss与性能优化第一阶段测试报告

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

   TimescaleDB是一个开源的时间序列数据库，用于高性能和可扩展的时间序列数据存储和分析。本次需求实现openGauss 5.1.1版本适配基于PostgreSQL9.6.3的TimescaleDB 1.7.4插件。

## 1.1 适配openGauss 5.1.1内核中缺失的TimescaleDB所依赖的扩展点
   根据开发交付件识别出结构体、函数、算子扩展点如下：
### 1.1.1 openGauss内核中缺失的TimescaleDB所依赖的结构体

| 序号 | 结构体                        | 说明                                                         |                                                              |
| ---- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | shm_mq                        | This structure represents the   actual queue, stored in shared memory. | 这个结构表示实际的队列，存储在共享内存中。                   |
| 2    | shm_mq_handle                 | This structure is a   backend-private handle for access to a queue. | 该结构体是后端私有句柄，用于访问队列。                       |
| 3    | BackgroundWorkerHandle        | Handle bgw                                                   | 处理bgw                                                      |
| 4    | dsm_segment                   | Backend-local state for a   dynamic shared memory segment.   | 动态共享内存段的后端本地状态。                               |
| 5    | shm_mq_iovec                  | Descriptors for a single write   spanning multiple locations. | 用于跨越多个位置的单个写入的描述符。                         |
| 6    | shm_mq_result                 | Possible results of a send or   receive operation.           | 发送或接收操作的可能结果。                                   |
| 7    | shm_toc                       | shared memory segment table of   contents                    | 共享内存段目录                                               |
| 8    | shm_toc_estimator             | Tools for estimating how large a   chunk of shared memory will be needed to store a TOC and its dependent   objects. | 用于估计存储TOC及其依赖对象需要多大的共享内存块的工具。      |
| 9    | dsm_handle                    | A "name" for a dynamic   shared memory segment.              | 动态共享内存段的“名称”。                                     |
| 10   | parallel_worker_main_type     | for launching parallel workers                               | 用于启动并行工作器                                           |
| 11   | HTSU_Result                   | Result codes for   HeapTupleSatisfiesUpdate.  This should   really be in tqual.h, but we want to avoid including that file elsewhere. | HeapTupleSatisfiesUpdate的结果代码                           |
| 12   | create_upper_paths_hook_type  | Hook for plugins to get control   when grouping_planner() plans upper rels | 当grouping_planner()计划上层rels时，插件获取控制权的钩子函数 |
| 13   | planner_hook_type_tsdb        | Hook for plugins to get control   in planner()               | 用于插件在计划器中获取控制权的钩子函数                       |
| 14   | OnConflictAction              | "ON CONFLICT" clause   type of query                         | 查询的“ON CONTRACE”子句类型                                  |
| 15   | CustomScanState               | CustomScan nodes are used to   execute custom code within executor. | CustomScan节点用于在executor中执行自定义代码。               |
| 16   | ParallelWorkerInfo            | Infrastructure for launching   parallel workers              | 用于启动并行工作进程的基础架构                               |
| 17   | ParallelContext               | Infrastructure for launching   parallel workers              | 用于启动并行工作进程的基础架构                               |
| 18   | CustomExecMethods             | Execution-time methods for a   CustomScanState.              | CustomScanState的执行时方法。                                |
| 19   | CustomPathMethods             | Custom path methods.                                         | 自定义路径方法。                                             |
| 20   | CustomPath                    | CustomPath represents a table   scan done by some out-of-core extension. | CustomPath表示由某个非核心扩展完成的表扫描。                 |
| 21   | OnConflictExpr                | OnConflictExpr - represents an   ON CONFLICT DO ... Expression | OnConflictExpr -表示一个ON CONFLICT表达式                    |
| 22   | ModifyTablePath               | ModifyTablePath represents   performing INSERT/UPDATE/DELETE modifications | ModifyTablePath表示执行INSERT/UPDATE/DELETE修改              |
| 23   | HeapUpdateFailureData         | When heap_update, heap_delete,   or heap_lock_tuple fail because the target tuple is already outdated, they   fill in this struct to provide information to the caller about what happened. | 当由于目标元组已经过时而导致堆更新、堆更新或堆锁更新失败时，它们将填充此结构体以向调用者提供有关信息。 |
| 24   | CustomScanState               | CustomScan nodes are used to   execute custom code within executor. | CustomScan节点用于在executor中执行自定义代码。               |
| 25   | CustomScanMethods             | Custom scan.  Here again, there's not much to do: we need   to be able to generate a ScanState corresponding to the scan. | 我们需要能够生成与扫描相对应的ScanState。                    |
| 26   | CheckEnableRlsResult          | Used by callers of   check_enable_rls.                       | check_enable_rls的调用者使用。                               |
| 27   | Result                        | If no outer plan, evaluate a   variable-free targetlist. If outer plan, return tuples from outer plan (after   a level of projection as shown by targetlist). | 如果没有外部计划，则评估一个无变量的目标列表。如果是外部计划，则从外部计划（在目标列表所示的投影级别之后）返回元组。 |
| 28   | AggSplit                      | Supported operating modes (i.e.,   useful combinations of these options): | 支持的操作模式（即这些选项的有效组合）                       |
| 29   | AggPath                       | AggPath represents generic   computation of aggregate functions | AggPath表示聚合函数的泛型计算                                |
| 30   | GatherPath                    | GatherPath runs several copies   of a plan in parallel and collects the results.  The parallel leader may also execute the   plan, unless the single_copy flag is set. | CollectPath并行运行计划的多个副本，并收集结果。并行leader也可以执行计划，除非设置了single_copy标志。 |
| 31   | MinMaxAggPath                 | MinMaxAggPath represents   computation of MIN/MAX aggregates from indexes | MinMaxAggPath表示从索引计算MIN/MAX聚合                       |
| 32   | RoleSpecType                  | RoleSpec - a role name or one of   a few special values.     | RoleSpecTypeRoleSpec -角色名称或几个特殊值之一。             |
| 33   | RoleSpec                      | RoleSpec - a role name or one of   a few special values.     | RoleSpec -角色名称或几个特殊值之一。                         |
| 34   | ReindexObjectType             | REINDEX Statement                                            | REINDEX声明                                                  |
| 35   | ProcessUtility_hook_type_tsdb | Hook for plugins to get control   in ProcessUtility()        | 用于插件在ProcessUtility中获取控制权的钩子函数               |
| 36   | BgWorkerStartTime             | Points in time at which a   bgworker can request to be started | bgworker可以请求启动的时间点                                 |
| 37   | ArrayBuildStateArr            | working state for   accumArrayResultArr() and friends note that the input must be arrays, and the   same array type is returned | accumArrayResultArr的工作状态（）和朋友们注意，输入必须是数组，并且返回的是相同的数组类型 |
| 38   | WindowAggPath                 | WindowAggPath represents generic   computation of window functions | WindowAggPath表示窗口函数的泛型计算                          |
| 39   | SortPath                      | represents an explicit sort step                             | 表示显式排序步骤                                             |
| 40   | ObjectAccessPostAlter         | Arguments of OAT_POST_ALTER   event                          | OAT_POST_ALTER事件的参数                                     |
| 41   | WCOKind                       | representation of WITH CHECK   OPTION checks to be applied to new tuples when inserting/updating an   auto-updatable view, or RLS WITH CHECK policies to be applied when   inserting/updating a relation with RLS. | 当插入/更新可自动更新的视图时，WITH CHECK OPTION检查将应用于新元组，或者当插入/更新与RLS的关系时，将应用于RLS WITH CHECK策略。 |
| 42   | RangeTblFunction              | RangeTblEntry subsidiary data   for one function in a FUNCTION RTE. | RangeTbl为函数RTE中的一个函数输入辅助数据。                  |
| 43   | InferenceElem                 | InferenceElem - an element of a   unique index inference specification | InferenceElem -唯一索引推理规范的元素                        |
| 44   | VacuumParams                  | Parameters customizing behavior   of VACUUM and ANALYZE.     | 自定义VACUUM和ANALYZE行为的参数。                            |
| 45   | BackgroundWorkerSlot          | BackgroundWorkerSlots exist in   shared memory and can be accessed (via the BackgroundWorkerArray) by both the   postmaster and by regular backends. | backgroundWorkerSlot存在于共享内存中，可以由postmaster和常规后端访问（通过backgroundWorkerArray）。 |
| 46   | BackgroundWorkerArray         | BackgroundWorkerSlots exist in   shared memory and can be accessed (via the BackgroundWorkerArray) by both the   postmaster and by regular backends. | backgroundWorkerSlot存在于共享内存中，可以由postmaster和常规后端访问（通过backgroundWorkerArray）。 |
| 47   | get_agg_clause_costs_context  | routines to manipulate   qualification clauses               | 操纵限定语句                                                 |
| 48   | IndexAMProperty               | Properties for amproperty API.                               | amproperty API的属性。                                       |
| 49   | CEOUC_WAIT_MODE               | waitMode argument to   check_exclusion_or_unique_constraint() | waitMode参数用于check_exclude_or_unique_constraint()         |
| 50   | XLTW_Oper                     | XactLockTableWait operations                                 | XactLockTableWait操作                                        |
| 51   | NamedLWLockTrancheRequest     | struct representing the LWLock   tranche request for named tranche | 表示命名段的LWLock段请求的结构                               |
| 52   | TimeoutId                     | Identifiers for timeout   reasons.                           | 超时原因的标识符。                                           |
| 53   | ObjectAccessPostCreate        | Arguments of OAT_POST_CREATE   event                         | OAT_POST_CREATE事件的参数                                    |
| 54   | Form_pg_policy                | pg_policy definition                                         | pg_policy定义                                                |
| 55   | EventTriggerCacheStateType    | Special-purpose cache for event   trigger data.              | 事件触发数据的专用缓存。                                     |
| 56   | ViewOptions                   | Contents of rd_options for views                             | 视图的rd_options的内容                                       |
| 57   | ResourceArray                 | ResourceArray is a common   structure for storing all types of resource IDs. | ResourceArray是一个通用的结构体，用于存储所有类型的资源ID。  |
| 58   | Form_pg_transform             | pg_transform definition                                      | pg_transform定义                                             |
| 59   | PQcommMethods                 | POSTGRES LIBPQ buffer structure   definitions.               | POSTGRES LIBPQ缓冲区结构定义。                               |
| 60   | xl_heap_lock_updated          | This is what we need to know   about locking an updated version of a row | 这是我们需要知道的关于锁定行的更新版本                       |
| 61   | has_parallel_hazard_arg       | routines to manipulate   qualification clauses               | 操纵限定语句                                                 |
| 62   | XactLockTableWaitInfo         | Struct to hold context info for   transaction lock waits.    | 用于保存事务锁等待的上下文信息的结构。                       |
| 63   | ProgressCommandType           | Command type for progress   reporting purposes               | 用于进度报告的命令类型                                       |
| 64   | WaitEvent                     | Bitmasks for events that may   wake-up WaitLatch(), WaitLatchOrSocket(), or WaitEventSetWait(). | 可能唤醒WaitLatch()、WaitLatchOrSocket()或WaitEventSetWait()的事件的位掩码。 |
| 65   | WaitEventSet                  | forward declaration to avoid   exposing latch.c implementation details | 前向声明以避免暴露latch.c实现细节                            |
| 66   | pairingheap_node              | This represents an element   stored in the heap. Embed this in a larger struct containing the actual data   you're storing. | 这表示存储在堆中的元素。将其嵌入到包含您要存储的实际数据的更大的结构体中。 |
| 67   | pairingheap                   | A pairing heap.                                              | 一个配对堆。                                                 |
| 68   | OldSnapshotControlData        | Structure for dealing with   old_snapshot_threshold implementation. | 用于处理old_snapshot_threshold实现的结构。                   |
| 69   | dsm_op                        | All the shared-memory operations we know   about.            | 所有共享内存操作                                             |
| 70   | VolatileFunctionStatus        | For volatile functions                                       | volatile函数                                                 |



### 1.1.2 openGauss内核中缺失的TimescaleDB所依赖的函数

| 序号 | 函数                                      | 说明                                                         |                                                              |
| ---- | ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | namecpy                                   | for comparison/operation   routines                          | 用于 比较/操作 路径                                          |
| 2    | RelationGetFKeyList                       | get a list of foreign key info   for the relation            | 获取关系的外键信息列表                                       |
| 3    | convert_tuples_by_name_map                | Return a palloc'd bare attribute   map for tuple conversion, matching input and output columns by name. | 返回一个用于元组转换的属性映射，按名称匹配输入和输出列。     |
| 4    | check_enable_rls                          | Determine, based on the   relation, row_security setting, and current role, if RLS is applicable to   this query. | 根据关系、row_security设置和当前角色确定RLS是否适用于此查询。 |
| 5    | PreventCommandIfParallelMode              | throw error if current   transaction is in parallel mode.    | 如果当前事务处于并行模式，则抛出错误。                       |
| 6    | CreateCacheMemoryContext                  | Standard routine for creating   cache context if it doesn't exist yet | 用于创建缓存上下文（如果缓存上下文尚不存在）的标准例程       |
| 7    | add_partial_path                          | Like add_path, our goal here is   to consider whether a path is worthy of being kept around, but the   considerations here are a bit different. A partial path is one which can be   executed in any number of workers in parallel such that each worker will   generate a subset of the path's overall result. | 和add_path一样，我们这里的目标是考虑路径是否值得保留，但这里的考虑有点不同。部分路径是可以在任意数量的worker中并行执行的路径，使得每个worker将生成路径整体结果的子集。 |
| 8    | create_agg_path                           | Creates a pathnode that   represents performing aggregation/grouping | 创建一个代表执行聚合/分组的路径节点                          |
| 9    | create_gather_path                        | Creates a path corresponding to   a gather scan, returning the pathnode. | 创建与聚集扫描对应的路径，返回路径节点。                     |
| 10   | get_agg_clause_costs                      | Recursively find the Aggref   nodes in an expression tree, and accumulate cost information about them. | 递归查找表达式树中的Aggref节点，并累加它们的代价信息。       |
| 11   | create_minmaxagg_path                     | Creates a pathnode that   represents computation of MIN/MAX aggregates | 创建一个代表MIN/MAX聚合计算的路径节点                        |
| 12   | SS_identify_outer_params                  | identify the Params available   from outer levels            | 识别外部可用的参数                                           |
| 13   | SS_charge_for_initplans                   | account for initplans in Path   costs & parallelism          | 在路径开销和并行度中计算initplans                            |
| 14   | get_rolespec_oid                          | Given a RoleSpec node, return   the OID it corresponds to.  If   missing_ok is true, return InvalidOid if the role does not exist. | 给定一个RoleSpec节点，返回它对应的OID。如果missing_ok为true，则如果角色不存在，则返回InvalidOid。 |
| 15   | ExecVacuum                                | Primary entry point for manual   VACUUM and ANALYZE commands | 手动VACUUM和ANALYZE命令的主入口点                            |
| 16   | BackgroundWorkerBlockSignals              | Block/unblock signals in a   background worker               | 在后台工作进程中阻止/解除阻止信号                            |
| 17   | BackgroundWorkerUnblockSignals            | Block/unblock signals in a   background worker               | 在后台工作进程中阻止/解除阻止信号                            |
| 18   | BackgroundWorkerInitializeConnectionByOid | Connect background worker to a   database using OIDs.        | 使用OID将后台工作者连接到数据库。                            |
| 19   | BackgroundWorkerInitializeConnection      | Connect background worker to a   database.                   | 将后台工作者连接到数据库。                                   |
| 20   | RegisterDynamicBackgroundWorker           | Register a new background worker   from a regular backend.   | 从常规后端注册一个新的后台工作者。                           |
| 21   | TerminateBackgroundWorker                 | Instruct the postmaster to   terminate a background worker.  | 指示邮局主管终止后台工作人员。                               |
| 22   | WaitForBackgroundWorkerStartup            | Wait for a background worker to   start up.                  | 等待后台工作者启动。                                         |
| 23   | GetBackgroundWorkerPid                    | Get the PID of a   dynamically-registered background worker. | 获取动态注册的后台工作者的PID。                              |
| 24   | WaitForBackgroundWorkerShutdown           | Wait for a background worker to   stop.                      | 等待后台工作者停止。                                         |
| 25   | ExplainPropertyBool                       | Explain a bool-valued property.                              | 解释bool值属性。                                             |
| 26   | slot_getallattrs                          | This function forces all the   entries of the slot's Datum/isnull arrays to be valid.  The caller may then extract data directly   from those arrays instead of using slot_getattr. | 此函数强制插槽的Datum/isnull数组的所有条目都是有效的。然后，调用者可以直接从这些数组中提取数据，而不是使用slot_getattr。 |
| 27   | mark_partial_aggref                       | Adjust an Aggref to make it   represent a partial-aggregation step. | 调整Aggref使其表示部分聚合步骤。                             |
| 28   | roleSpecsToIds                            | Given a list of RoleSpecs,   generate a list of role OIDs in the same order. | 给定一个RoleSpec列表，以相同的顺序生成一个角色OID列表。      |
| 29   | GetNamedLWLockTranche                     | returns the base address of   LWLock from the specified tranche. | 从指定的块中返回LWLock的基地址。                             |
| 30   | RequestNamedLWLockTranche                 | Request that extra LWLocks be   allocated during postmaster startup. | 请求在postmaster启动期间分配额外的LWLock。                   |
| 31   | heap_beginscan_catalog                    | heap_beginscan_catalog differs   in setting up its own temporary snapshot. | stack_beginscan_catalog的不同之处在于设置自己的临时快照。    |
| 32   | toast_get_valid_index                     | Get OID of valid index   associated to given toast relation. A toast relation can have only one valid   index at the same time. | 获取与给定toast 关联的有效索引的OID。一个toast   在同一时间只能有一个有效的索引。 |
| 33   | reindex_relation                          | This routine is used to recreate   all indexes of a relation (and optionally its toast relation too, if any). | 此例程用于重新创建关系的所有索引（以及可选地，如果有）。     |
| 34   | MemoryContextAllocExtended                | Allocate space within the   specified context using the given flags. | 使用给定的标志在指定的上下文中分配空间。                     |
| 35   | initArrayResultArr                        | initialize an empty   ArrayBuildStateArr                     | 初始化一个空的ArrayBuildStateArr                             |
| 36   | initArrayResult                           | initialize an empty   ArrayBuildState                        | 初始化一个空的ArrayBuildState                                |
| 37   | accumArrayResultArr                       | accumulate one (more) sub-array   for an array result        | 为数组结果累加一个（多个）子数组                             |
| 38   | makeArrayResultArr                        | produce N+1-D final result of   accumArrayResultArr          | 生成accumArrayResultArr的N+1-D最终结果                       |
| 39   | check_index_predicates                    | Set the predicate-derived   IndexOptInfo fields for each index of the specified relation. | 为指定关系的每个索引设置predicate-derived的IndexOptInfo字段。 |
| 40   | create_sort_path                          | Creates a pathnode that   represents performing an explicit sort. | 创建一个表示执行显式排序的路径节点。                         |
| 41   | IsInParallelMode                          | Check this to prohibit   operations that change backend-local state expected to match across all   workers. | 选中此选项可禁止更改后端本地状态的操作，以在所有工作线程中匹配。 |
| 42   | RunObjectPostCreateHook                   | Hook on object accesses.  This is intended as infrastructure for   security and logging plugins. | 钩子函数，用于对象访问。它旨在作为安全和日志插件的基础设施。 |
| 43   | InitPostgres                              | Initialize POSTGRES.                                         | 初始化POSTGRES。                                             |
| 44   | cost_gather                               | Determines and returns the cost   of gather path.            | 确定并返回聚集路径的开销。                                   |
| 45   | has_parallel_hazard                       | Check whether a node tree   contains parallel hazards.       | 检查节点树是否包含并行危险。                                 |
| 46   | has_bypassrls_privilege                   | Superusers bypass all permission   checking                  | 超级用户绕过所有权限检查                                     |
| 47   | InNoForceRLSOperation                     | are we ignoring FORCE ROW LEVEL   SECURITY ?                 | 我们是否忽视了原始数据安全                                   |
| 48   | SpeculativeInsertionWait                  | Wait for the specified   transaction to finish or abort the insertion of a tuple. | 等待指定的事务完成或中止元组的插入。                         |
| 49   | slot_getsomeattrs                         | This function forces the entries   of the slot's Datum/isnull arrays to be valid at least up through the   attnum'th entry. | 该函数强制槽的Datum/isnull数组的条目至少在第attnum'个条目之前有效。 |
| 50   | ResourceOwnerEnlargeDSMs                  | Make sure there is room for at   least one more entry in a ResourceOwner's dynamic shmem segment reference   array. | 确保ResourceOwner的动态shmem段引用数组中至少还有一个条目。   |
| 51   | ResourceOwnerRememberDSM                  | Remember that a dynamic shmem   segment is owned by a ResourceOwner | 记住动态shmem段归ResourceOwner所有                           |
| 52   | ResourceOwnerForgetDSM                    | Forget that a dynamic shmem   segment is owned by a ResourceOwner | 丢弃动态shmem段归ResourceOwner所有                           |
| 53   | check_functions_in_node                   | apply checker() to each function   OID contained in given expression node | 对给定表达式节点中包含的每个函数OID应用检查器()              |
| 54   | func_parallel                             | Given procedure id, return the   function's proparallel flag. | 给定过程id，返回函数的proparallel标志。                      |
| 55   | find_childrel_parents                     | Compute the set of parent relids   of an appendrel child rel. | 计算appendrel子rel的父依赖的集合。                           |
| 56   | shm_mq_create                             | Initialize a new shared message   queue.                     | 初始化一个新的共享消息队列。                                 |
| 57   | shm_mq_set_receiver                       | Set the identity of the process   that will receive from a shared message queue. | 设置将从共享消息队列接收的进程的标识。                       |
| 58   | shm_mq_get_receiver                       | Get the configured receiver.                                 | 获取配置的接收器。                                           |
| 59   | shm_mq_get_sender                         | Get the configured sender.                                   | 获取配置的发件人。                                           |
| 60   | shm_mq_set_sender                         | Set the identity of the process   that will send to a shared message queue. | 设置将发送到共享消息队列的进程的标识。                       |
| 61   | shm_mq_attach                             | Attach to a shared message queue   so we can send or receive messages. | 附加到一个共享的消息队列，这样我们就可以发送或接收消息了。   |
| 62   | shm_mq_set_handle                         | Associate a   BackgroundWorkerHandle with a shm_mq_handle just as if it had been passed to   shm_mq_attach. | 将BackgroundWorkerHandle与shm_mq_handle关联，就像它已经被传递给shm_mq_attach一样。 |
| 63   | shm_mq_detach                             | Detach a shared message queue.                               | 分离共享消息队列。                                           |
| 64   | shm_mq_get_queue                          | Get the shm_mq from handle.                                  | 从句柄获取shm_mq。                                           |
| 65   | shm_mq_send                               | Write a message into a shared   message queue.               | 将消息写入共享消息队列。                                     |
| 66   | shm_mq_sendv                              | Write a message into a shared   message queue, gathered from multiple addresses. | 将消息写入从多个地址收集的共享消息队列中。                   |
| 67   | shm_mq_receive                            | Receive a message from a shared   message queue.             | 从共享消息队列接收消息。                                     |
| 68   | shm_mq_wait_for_attach                    | Wait for the other process   that's supposed to use this queue to attach to it. | 等待本应使用此队列的另一个进程连接到它。                     |
| 69   | shm_toc_create                            | Initialize a region of shared   memory with a table of contents. | 使用目录初始化共享内存区域。                                 |
| 70   | shm_toc_attach                            | Attach to an existing table of   contents.  If the magic number found at   the target address doesn't match our expectations, returns NULL. | 附加到现有目录。如果在目标地址找到的幻数与我们的期望值不匹配，则返回NULL。 |
| 71   | shm_toc_allocate                          | Allocate shared memory from a   segment managed by a table of contents. | 从目录管理的段中分配共享内存。                               |
| 72   | shm_toc_freespace                         | Return the number of bytes that   can still be allocated.    | 返回仍可分配的字节数。                                       |
| 73   | shm_toc_insert                            | Insert a TOC entry.                                          | 插入TOC条目。                                                |
| 74   | shm_toc_lookup                            | Look up a TOC entry.                                         | 查找TOC条目。                                                |
| 75   | dsm_impl_op                               | Perform a low-level shared   memory operation in a platform-specific way, as dictated by the selected   implementation.  Each implementation is   required to implement the following primitives. | 按照所选实现的规定，以特定于平台的方式执行低级共享内存操作。每个实现都需要实现以下原语。 |
| 76   | dsm_impl_can_resize                       | Does the current dynamic shared   memory implementation support resizing segments? | 当前的动态共享内存实现是否支持调整段的大小？                 |
| 77   | dsm_impl_pin_segment                      | Implementation-specific actions   that must be performed when a segment is to be preserved until postmaster   shutdown. | 当段要保留到postmaster关闭时必须执行的特定于实现的操作。     |

### 1.1.3 openGauss内核中缺失的TimescaleDB所依赖的算子

| 序号 | 算子                  | 说明                                                         |                                                              |
| ---- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | T_ModifyTablePath     | ModifyTablePath represents   performing INSERT/UPDATE/DELETE modifications | ModifyTablePath表示执行INSERT/UPDATE/DELETE修改              |
| 2    | T_AggPath             | AggPath represents generic computation of aggregate functions | AggPath表示聚合函数的泛型计算                                |
| 3    | T_WindowAggPath       | WindowAggPath represents generic computation of window   functions | WindowAggPath表示窗口函数的泛型计算                          |
| 4    | T_SortPath            | represents an explicit sort step                             | 表示显式排序步骤                                             |
| 5    | T_MinMaxAggPath       | MinMaxAggPath represents   computation of MIN/MAX aggregates from indexes | MinMaxAggPath表示从索引计算MIN/MAX聚合                       |
| 6    | T_GatherPath          | GatherPath runs several copies   of a plan in parallel and collects the results.  The parallel leader may also execute the   plan, unless the single_copy flag is set. | CollectPath并行运行计划的多个副本，并收集结果。并行leader也可以执行计划，除非设置了single_copy标志。 |
| 7    | T_ForeignKeyCacheInfo | Information the relcache can cache about foreign key   constraints | relcache可以缓存有关外键约束的信息                           |
| 8    | T_Gather              | gather node                                                  | 聚集节点                                                     |

## 1.2 已适配接口统计

​         PostgreSQL9.6.3+TimescaleDB 1.7.4合计接口55个。openGauss适配TimescaleDB插件，涉及35个接口无法适配(明细见：openGauss无法适配TimescaleDB接口表格)，可适配接口20个，第一阶段适配接口11个，第一阶段适配的接口占可适配接口的比例为55%。

### 1.2.1 TimescaleDB一阶段已实现接口

| 序号 | 接口名称                           |
| ---- | ---------------------------------- |
| 1    | add_dimension()                    |
| 2    | attach_tablespace（）              |
| 3    | create_hypertable（）              |
| 4    | detach_tablespace（）              |
| 5    | detach_tablespaces（）             |
| 6    | set_chunk_time_interval（）        |
| 7    | set_integer_now_func（）           |
| 8    | time_bucket()                      |
| 9    | timescaledb_information.hypertable |
| 10   | timescaledb_information.license    |
| 11   | show_tablespaces（）               |

### 1.2.2 TimescaleDB二阶段待实现接口

| 序号 | 接口名称                        | 所属类别 |
| ---- | ------------------------------- | -------- |
| 1    | chunk_relation_size             | chunk    |
| 2    | chunk_relation_size_pretty      | chunk    |
| 3    | drop_chunks                     | chunk    |
| 4    | hypertable_relation_size        | chunk    |
| 5    | hypertable_relation_size_pretty | chunk    |
| 6    | indexes_relation_size           | chunk    |
| 7    | indexes_relation_size_pretty    | chunk    |
| 8    | set_number_partitions           | chunk    |
| 9    | show_chunks                     | chunk    |

### 1.2.3 openGauss无法适配TimescaleDB接口

| 序号 | 接口名称                                            | 所属类别 |
| ---- | --------------------------------------------------- | -------- |
| 1    | add_drop_chunks_policy                              | policy   |
| 2    | add_reorder_policy                                  | policy   |
| 3    | add_compress_chunks_policy                          | policy   |
| 4    | alter_job_schedule                                  | policy   |
| 5    | remove_compress_chunks_policy                       | policy   |
| 6    | remove_drop_chunks_policy                           | policy   |
| 7    | remove_reorder_policy                               | policy   |
| 8    | timescaledb_information.drop_chunks_policies        | policy   |
| 9    | timescaledb_information.policy_stats                | policy   |
| 10   | timescaledb_information.reorder_policies            | policy   |
| 11   | alter   view (continuous aggregate)                 | 聚合函数 |
| 12   | create   view (continuous aggregate)                | 聚合函数 |
| 13   | drop   view (continuous aggregate)                  | 聚合函数 |
| 14   | first                                               | 聚合函数 |
| 15   | get_telemetry_report                                | 聚合函数 |
| 16   | interpolate                                         | 聚合函数 |
| 17   | last                                                | 聚合函数 |
| 18   | locf                                                | 聚合函数 |
| 19   | refresh   materialized view (continuous aggregate)  | 聚合函数 |
| 20   | time_bucket_gapfill                                 | 聚合函数 |
| 21   | timescaledb_information.continuous_aggregates       | 聚合函数 |
| 22   | timescaledb_information.continuous_aggregate_stats  | 聚合函数 |
| 23   | histogram                                           | 聚合函数 |
| 24   | timescaledb_information.compressed_chunk_stats      | chunk    |
| 25   | timescaledb_information.compressed_hypertable_stats | chunk    |
| 26   | timescaledb.license_key                             | chunk    |
| 27   | move_chunk                                          | chunk    |
| 28   | reorder_chunk                                       | chunk    |
| 29   | hypertable_approximate_row_count                    | chunk    |
| 30   | compress_chunk                                      | chunk    |
| 31   | decompress_chunk                                    | chunk    |
| 32   | create index (transaction per chunk)                | chunk    |
| 33   | alter table (compression)                           | 压缩     |
| 34   | timescaledb_pre_restore                             | 备份     |
| 35   | timescaledb_post_restore                            | 备份     |

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 3172134f | 2023-11-09   | 2023-12-20   |

| 硬件型号          | 硬件配置信息                                                 | 备注 |
| ----------------- | ------------------------------------------------------------ | ---- |
| x86_64+centOS     | Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz<br/>内存：755GB<br/>硬盘：1T<br/>OS：CentOS Linux release 7.9.2009 (Core) |      |
| aarch64+openEuler | CPU：kunpeng-920 2600MHz<br/>内存：765GB<br/>硬盘：3.0TB<br/>OS：openEuler release 20.03 (LTS-SP1) |      |

# 3     测试结论概述

## 3.1   测试整体结论

​       timescaledb适配openGauss与性能优化第一阶段实现接口合计11个,其中6个与第二阶段实现的chunk功能存在交互（资料中已加约束），待第二阶段转测后测试涉及到与chunk交互的功能。第一阶段累计发现缺陷17个，其中已验收16个、修复中1个(遗留问题)，总体质量一般。

## 3.2   约束说明

- 在兼容pg库下创建插件
- 不支持非编译安装版本
- 目前TimescaleDB安装之后，不支持删除TimescaleDB插件
- TimescaleDB插件依赖于public schema，因此不支持使用drop schema的方式删除public schema
- chunk功能不支持
- 在不同数据库创建插件需要重启数据库

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| [I8MRMC](https://gitee.com/opengauss/Plugin/issues/I8MRMC?from=project-issue)       |  timescaleDB 插件不支持同时在两个库中创建          |

遗留问题导致无法在两个pg兼容库下分别创建TimescaleDB插件。规避措施：在不同pg库下创建timescaleDB插件前需要先重启数据库，已在资料中加说明。

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 17       | 1    | 5    | 9    | 2      |
| 百分比 | 100%     | 6%   | 30%  | 53%  | 11%    |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I8FEGN](https://gitee.com/opengauss/Plugin/issues/I8FEGN?from=project-issue) | 严重     | timescaleDB创建超表失败                                      | 已验收   |
| 2    | [I8H6PJ](https://gitee.com/opengauss/Plugin/issues/I8H6PJ?from=project-issue) | 主要     | timescaleDB插件在aarch64架构、openEuler release 20.03操作系统将标准表转化为超表失败 | 已验收   |
| 3    | [I8H6XI](https://gitee.com/opengauss/Plugin/issues/I8H6XI?from=project-issue) | 主要     | timescaleDB指定两个时间列创建超表失败                        | 已验收   |
| 4    | [I8HBKC](https://gitee.com/opengauss/Plugin/issues/I8HBKC?from=project-issue) | 主要     | timescaleDB自定义模式名创建超表失败                          | 已验收   |
| 5    | [I8IECT](https://gitee.com/opengauss/Plugin/issues/I8IECT?from=project-issue) | 次要     | timescaleDB chunk_time_interval 参数可通过 INTERVAL '0 day'方式设置为0，未合理报错 | 已验收   |
| 6    | [I8HX1M](https://gitee.com/opengauss/Plugin/issues/I8HX1M?from=project-issue) | 主要     | timescaleDB 标准表指定表空间后创建超表失败                   | 已验收   |
| 7    | [I8JT6P](https://gitee.com/opengauss/Plugin/issues/I8JT6P?from=project-issue) | 不重要   | timescaleDB 查询许可证信息timescaledb_information.license报错 | 已验收   |
| 8    | [I8HEAG](https://gitee.com/opengauss/Plugin/issues/I8HEAG?from=project-issue) | 次要     | timescaleDB 删除超表后，系统表_timescaledb_catalog.hypertable中该超表的信息存在 | 已验收   |
| 9   | [I8JYI8](https://gitee.com/opengauss/Plugin/issues/I8JYI8?from=project-issue) | 次要     | timescaleDB set_chunk_time_interval()函数通过interval 设置时间间隔，默认单位为天，应为秒 | 已验收   |
| 10   | [I8NGQF](https://gitee.com/opengauss/Plugin/issues/I8NGQF?from=project-issue) | 次要     | timescaleDB 系统函数 _timescaledb_internal.get_partition_for_key()获取的值不准确 | 已验收   |
| 11   | [I8NHP9](https://gitee.com/opengauss/Plugin/issues/I8NHP9?from=project-issue) | 次要     | timescaleDB 表非超表，系统函数_timescaledb_internal.get_create_command()提示信息不准确 | 已验收   |
| 12   | [I8NOM4](https://gitee.com/opengauss/Plugin/issues/I8NOM4?from=project-issue) | 次要     | timescaleDB 补充包括插件安装步骤、已实现接口、约束及操作示例资料 | 已验收   |
| 13   | [I8NUT9](https://gitee.com/opengauss/Plugin/issues/I8NUT9?from=project-issue) | 主要     | timescaleDB 插件的系统函数、系统视图、系统表、序列涉及46个未适配 | 已验收   |
| 14   | [I8QMIL](https://gitee.com/opengauss/Plugin/issues/I8QMIL?from=project-issue) | 次要     | timescaleDB 创建插件报错                                     | 已验收   |
| 15   | [I8R5JC](https://gitee.com/opengauss/Plugin/issues/I8R5JC?from=project-issue) | 不重要   | timescaleDB 超表已实现不用cascade即可删除，去掉资料中的约束加cascade，第二阶段实现的功能代码尚未合入资料暂不体现 | 已验收   |
| 16   | [I8R610](https://gitee.com/opengauss/Plugin/issues/I8R610?from=project-issue) | 主要     | timescaleDB 在执行sql语句的过程中会出现卡住的情况            | 已验收   |

# 5     测试执行

## 5.1 测试执行步骤

### 5.1.1 功能测试

| 接口名称                           | 功能介绍                           | 测试步骤：                                                   | 测试结果                                                     |
| ---------------------------------- | ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| add_dimension()                    | 向超表添加额外的分区维度           | 1.创建超表<br/>2.覆盖可选参数与必选参数，验证添加超表的额外分区成功<br/> | 1.执行12条用例<br/>2.提交issue 1个，均已验收<br/>3.现已修复且验收通过<br/>4.与第二阶段的chunk功能有交互，比如在插入数据后在添加额外的分区维度时，应该合理报错hypertable "test_table" has tuples or empty chunks。但我们第一阶段的程序没有实现chunk功能(chunk未实现的在资料中加说明) |
| attach_tablespace()                | 将表空间附加到超表并使用它来存储块 | 1.创建超表<br/>2.验证表空间存在、不存在的场景，及参数的验证<br/> | 1.执行8条用例<br/>2.未发现bug<br/>3.与第二阶段chunk功能有交互，第二阶段待转测。 |
| create_hypertable()                | 创建超表                           | 1.验证date、timestamp、timestamptz、整型作为时间列创建超表成功<br/>2.参数的验证<br/>3.创建超表后返回值的验证 | 1.执行45条用例 <br/>2.提交issue 8个，均已验收<br/>3.参数migrate_data与第二阶段chunk功能有交互，第二阶段待转测 |
| detach_tablespace()                | 从一个或多个超级表中分离表空间     | 1.验证待分离表空间存在、不存在、多个超表的表空间分离场景验证<br>2.参数的验证 | 1.执行6条用例<br/>2.提交issue 1个，已验收<br/>3.第二阶段chunk功能有交互，第二阶段待转测。如分离表空间后，新的chunk将被放置在数据库的默认表空间中 |
| detach_tablespaces()               | 从超表中分离所有表空间             | 1.验证待分离超表下所有的表空间<br/>2.参数的验证              | 1.执行1条用例<br/>2.未发现bug<br/>3.第二阶段chunk功能有交互，第二阶段待转测。如分离表空间后，新的chunk将被放置在数据库的默认表空间中 |
| set_chunk_time_interval()          | 设置超表上的区块时间间隔           | 1.分别验证date、timestamp、timestamptz、整型作为时间分区时更改时间间隔<br>2.函数入参的验证 | 1.执行16条用例<br/>2.提交issue 2个，均已验收                 |
| set_integer_now_func()             | 设置整数超表当前时间函数           | 1.验证自定义函数返回当前时间值的场景<br>2.函数入参的验证     | 1.执行4条用例<br/>2.未发现bug                                |
| time_bucket()                      | 用于分析任意时间间隔的数据         | 1.验证weeks、day、hours为时间间隔，进行统计数据<br>2.函数入参的验证 | 1.执行28条用例<br/>2.未发现bug                               |
| timescaledb_information.hypertable | 获取超表的相关信息                 | 1.超表创建成功后查看信息是否写入系统表中                     | 1.执行3条用例<br/>2.未发现bug<br/>3.超表信息与第二阶段chunk功能有交互，第二阶段待转测 |
| timescaledb_information.license    | 获取有关当前许可证的信息           | 1.成功获取当前许可证信息                                     | 1.执行1条用例<br/>2.提交issue1个，均已验收                   |
| show_tablespaces()                 | 将显示附加到超表的表空间           | 1.验证超表存在多少个表空间                                   | 1.执行1条用例<br/>2.未发现bug                                |

### 5.1.2 插件引入的系统函数、系统表、系统视图测试

| 系统函数                                            | 功能介绍                                                | 测试结果              |
| --------------------------------------------------- | ------------------------------------------------------- | --------------------- |
| _timescaledb_internal.current_license_key           | 获取当前许可证密钥                                      | 通过                  |
| _timescaledb_internal.license_expiration_time       | 许可证到期时间                                          | 通过                  |
| _timescaledb_internal.print_license_expiration_info | 许可证到期信息                                          | 通过                  |
| _timescaledb_internal.license_edition               | 版本信息                                                | 通过                  |
| _timescaledb_internal.to_unix_microseconds          | 时间类型转微秒                                          | 通过                  |
| _timescaledb_internal.to_timestamp                  | 微秒转化为带时区时间                                    | 通过                  |
| _timescaledb_internal.to_timestamp_without_timezone | 微秒转化为不带时区时间                                  | 通过                  |
| _timescaledb_internal.to_date                       | 微秒转化为date格式日期                                  | 通过                  |
| _timescaledb_internal.to_interval                   | 微秒转化为时间间隔显示                                  | 通过                  |
| _timescaledb_internal.time_literal_sql              | 微秒转化为指定的时间间隔类型                            | 通过                  |
| _timescaledb_internal.dimension_is_finite           | 维护和管理时间序列数据分区,优化时间序列数据的存储和查询 | 通过                  |
| _timescaledb_internal.get_time_type                 | 获取这个时间列的数据类型                                | 通过                  |
| _timescaledb_internal.is_main_table                 | 用于区分TimescaleDB 中的超表和其他类型的表              | 通过                  |
| _timescaledb_internal.main_table_from_hypertable    | 输入超表id返回超表表名称                                | 通过                  |
| _timescaledb_internal.get_git_commit                | 查看插件版本                                            | 通过                  |
| _timescaledb_internal.get_os_info                   | 查看操作系统信息                                        | 通过                  |
| _timescaledb_internal.range_value_to_pretty         | 将如时间范围、数值范围等转换为更易读或标准化的格式      | 通过                  |
| _timescaledb_internal.generate_uuid                 | 函数返回随机的uuid                                      | 通过                  |
| _timescaledb_internal.partitioning_column_to_pretty | 返回超表时间分区                                        | 通过                  |
| _timescaledb_internal.valid_ts_interval             | 用于验证时间间隔是否有效                                | 通过                  |
| _timescaledb_internal.get_create_command            | 查看创建超表的命令                                      | 通过                  |
| _timescaledb_internal.get_partition_hash            | 获取分区返回值                                          | 通过                  |
| _timescaledb_internal.get_partition_for_key         | 获取分区返回值                                          | 提交issue 1个，已验收 |

### 5.1.3 TimescaleDB 1.7.4 官方fastcheck用例执行

合计151个用例，测试用例的sql语法需要更改适配openGauss。第一阶段实现的功能仅支持5个用例的执行，5个通过，提交issue1个，已验收

| 用例名称              | 测试点                                                       | 执行结果             |
| --------------------- | ------------------------------------------------------------ | -------------------- |
| create_table.out      | 测试普通表有主键约束转化为超表<br/>以及不支持引用超级表的外键约束 | 通过                 |
| generated_columns.out | 普通表包含生成列转为超表                                     | 通过                 |
| hash.out              | 测试系统函数_timescaledb_internal.get_partition_hash 与 _timescaledb_internal.get_partition_for_key的分区返回值 | 提交issue1个，已验收 |
| version.out           | 测试系统函数_timescaledb_internal.get_os_info()  _timescaledb_internal.get_git_commit()返回的操作系统、插件版本信息为文本信息 | 通过                 |
| plain.out             | 测试openGauss的sql语句，以确保它们在加载TimescaleDB扩展时正常工作 | 通过                 |



### 5.1.4资料测试

| 测试步骤：                               | 测试结果               |
| ---------------------------------------- | ---------------------- |
| 1.校验资料的描述及示例的执行结果是否成功 | 提交issue2个，均已验收 |



## 5.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 5.1.1 build 3172134f | 150        | Passed：130<br>Failed：20 | 17           |
| openGauss 5.1.1 build 4df8f86c | 20         | Passed：20<br/>Failed：0  | 0            |

*数据项说明：*

- 第一阶段累计发现缺陷单17个，已验收16个，修复中1个（遗留问题）
- 缺陷密度为17个(缺陷个数)/71.435kloc(代码行数)=0.238(个/kloc)

## 5.3   后续测试建议

第二阶段代码转测后，测试第一阶段涉及到chunk功能的函数（未实现的chunk功能在资料中加说明）

# 6     附件

```sql 
test1=# create extension timescaledb;  --创建插件
WELCOME TO
 _____ _                               _     ____________  
|_   _(_)                             | |    |  _  \ ___ \ 
  | |  _ _ __ ___   ___  ___  ___ __ _| | ___| | | | |_/ / 
  | | | |  _ ` _ \ / _ \/ __|/ __/ _` | |/ _ \ | | | ___ \ 
  | | | | | | | | |  __/\__ \ (_| (_| | |  __/ |/ /| |_/ /
  |_| |_|_| |_| |_|\___||___/\___\__,_|_|\___|___/ \____/
               Running version 1.7.4
For more information on TimescaleDB, please visit the following links:

 1. Getting started: https://docs.timescale.com/getting-started
 2. API reference documentation: https://docs.timescale.com/api
 3. How TimescaleDB is designed: https://docs.timescale.com/introduction/architecture

Note: TimescaleDB collects anonymous reports to better understand and assist our users.
For more information and how to disable, please see our docs https://docs.timescaledb.com/using-timescaledb/telemetry.
CREATE EXTENSION

CREATE TABLE conditions (
  time        TIMESTAMPTZ       NOT NULL,
  location    TEXT              NOT NULL,
  temperature DOUBLE PRECISION  NULL,
  humidity    DOUBLE PRECISION  NULL
);

-- 创建超表
SELECT create_hypertable('conditions', 'time');

```

