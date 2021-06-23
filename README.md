Python Trigger
====
This is not mine, nor do I take any damn credit for it, Google translated some shit, and thats about it...so use how ever you feel, and if it breaks, or does not work as intended, well, I am not your guy (lazy as hell, I know, but...whatever.  I at least own up to it).  The translations for each section will be posted under the bullet points, and if you are able to provide a better translation, feel free.  


Introduction
--

> * Python Trigger is a trigger framework written in python that provides a trigger framework for triggering other events based on event callbacks. It does not provide any independent functions by itself, but it can save you the time of writing function plug-in structures.

> * You can choose to write plug-ins in any language you are familiar with-as long as you can connect to the python interface at the end

Hello World
--

> * The plugin of Python Trigger initially has three events: plugin_onload, plugin_onunload and plugin_start, which are implemented by inheriting FFxivPythonTrigger.PluginBase 


> Event | Use
> --- |:---:

> * Through the FPT attribute of PluginBase, you can access some convenient functions provided by the framework

> * plugin_onload| Plug-in initialization (__init__ is not recommended as initialization)


> *  plugin_start | Start running plug-in logic after successful initialization (usually used to suspend the process) (need to be written in async mode)

> * plugin_onunload| Plug-in uninstallation (close the running process, clean up and store data) 

> * At the same time, please set the name of the plug-in as the purpose of plug-in identification

> * Please note that the process that hangs in plugin_onload may block other plugin initialization or the main process will not wait for its completion before ending. Therefore, it is recommended to use plugin_start to operate the thread that is maintained for a long time.

> * In order to provide interoperability between plug-ins, provide plug-in-level API registration and use
> * self.FPT.api：
> 
> Function | Use
> --- |:---:
> register_attribute(name:string,object:any)| Register api so that other programs can call
> * After creation, other plug-ins can access the object through'self.FPT.api. registered name'
> * The api registered through this method will be uninstalled at the same time after the plugin is uninstalled

> * In order to provide the persistence of plug-in data, a storage method is provided for standardized storage
> * self.FPT.storage：
>  Functions, Variables | Use
> --- |:---:
> * data| Persistent storage entity, dict attribute
> * path| The folder path assigned by the plugin
>> load()| Load json data to data (automatically called when the plugin is loaded)
> store()| Store the data json after processing (automatically invoked by plugin uninstallation)
> 
> * In order to normalize the output of the program, log function is provided
> * self.FPT:
>
> Function | Use
> --- |:---:
> log(msg:string,level:number=logging.info)| Print the log at print_level or above of the level logger (the default is logging.info) and write it into the log file (seems not written yet)

> * Plug-in functions can be roughly divided into two types: one, create an event, and two, respond to an event
> * self.FPT:
>
> Function | Use
> --- |:---:
> process_event(event:FFxivPythonTrigger.EventBase)| Pass in an event object and distribute it through the id attribute of the event
> register_event(event_id:any, callback:callable)| Register a callback of the event id, the callback will be passed into the event object, it is recommended to be async function
> * The registered event callback will be uninstalled at the same time after the plugin is uninstalled
