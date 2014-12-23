# task
`Task` is an [Elixir Task](http://elixir-lang.org/getting_started/mix_otp/8.html#8.2-tasks) model partial implement in Erlang.

Detailed doc, [here](https://github.com/elixir-lang/elixir/blob/v1.0.1/lib/elixir/lib/task.ex).

## async and await

In this repo, the way to spawn a task is with `task:async/3`. Then, a new process will be created, linked and monitored by the caller(that is the task owner). Once the task action finishes, a message will be sent to the caller with the result.

`task:await/1` and `task:await/2` is used to read the message sent bt the task process. On `await`, `Task` will also setup a monitor to verify if the process exited for any abnormal reason(or in case exits are being trapped by the caller). 

## usage
### compile and eunit
	$ ./rebar com ; ./rebar eunit
	==> task (compile)
	==> task (eunit)
	
	=ERROR REPORT==== 23-Dec-2014::13:52:31 ===
	** Task <0.54.0> terminating
	** Started from <0.46.0>
	** When function == #Fun<erlang.throw.1>
	**      arguments == [unknown]
	** Reason for termination == 
	** {{nocatch,unknown},
	    [{task,do_apply,2,[{file,"src/task.erl"},{line,69}]},
	     {task,async_do,3,[{file,"src/task.erl"},{line,49}]},
	     {proc_lib,init_p_do_apply,3,[{file,"proc_lib.erl"},{line,239}]}]}

	=ERROR REPORT==== 23-Dec-2014::13:52:31 ===
	** Task <0.56.0> terminating
	** Started from <0.46.0>
	** When function == #Fun<erlang.exit.1>
	**      arguments == [unknown]
	** Reason for termination == 
	** unknown
	  All 7 tests passed.
	Cover analysis: ~/workspace/task/.eunit/index.html
	
### start
	$ erl -pa ./ebin
	
### e.g.

	1> task:async(erlang, self, []).
	{<0.34.0>,#Ref<0.0.0.30>}
	
	2> task:await(v(1)).
	<0.34.0>
	
	3> task:async(erlang, process_info, [erlang:self()]).
	{<0.37.0>,#Ref<0.0.0.39>}
	
	4> task:await(v(3)).
	[{current_function,{io,wait_io_mon_reply,2}},
	 {initial_call,{erlang,apply,2}},
	 {status,waiting},
	 {message_queue_len,0},
	 {messages,[]},
	 {links,[<0.26.0>,<0.37.0>]},
	 {dictionary,[]},
	 {trap_exit,false},
	 {error_handler,error_handler},
	 {priority,normal},
	 {group_leader,<0.25.0>},
	 {total_heap_size,986},
	 {heap_size,376},
	 {stack_size,29},
	 {reductions,2153},
	 {garbage_collection,[{min_bin_vheap_size,46422},
	                      {min_heap_size,233},
	                      {fullsweep_after,65535},
	                      {minor_gcs,6}]},
	 {suspending,[]}]
	 