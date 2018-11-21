---
title: activiti-基础
date: 2018-08-18 16:20:22
updated: 2018-08-18 16:20:22
tags: event
categories: activiti
comments: true
---

### 规范

#### 分类

* 启动与结束事件 (start &end event)
* 顺序流 (sequence flow)
* 任务 (task)
* 网关(geteway)
* 子流程(subprocess)
* 边界事件(boundary event)
* 中间事件(intermediate event)
* 监听器(listener)

<!--more-->

#### 启动与结束事件

>  启动事件：细圆圈表示，捕获型(等待触发：runtimeService.startProcessInstance)

* **空启动事件**：基本的启动事件，空的细圆圈符号

  * activiti扩展空启动属性

  * activiti:fromKey，记录空启动事件关联的表单文件

    * 外置表单：表单内容都存放在单独的.form 文件中，activiti:formkey 就用来指定那个表单文件

    * 动态表单：此类型不在用activiti:formkey来指定，而是在流程定义文件中设置表单元素集合

      ``` xml
      <activiti:formProperty id="name" name="name" type="string">/aciviti:formProperty>
      ```

  * activiti:initiator，记录启动流程人ID

* **定时启动事件**：一次性启动，特定时间间隔启动，圆圈中时钟符号

  * 在基本的启动事件中嵌套了一个定时事件定义timerEventDefinition

    ``` xml
    <startEvent id="timerstartevent" name="timer start">
        <timerEventDefinition>
        <!--定时日期类型-->
            <timeCycle>用ISO 8601标志表示</timeCycle>
        </timeEventDefinition>
    </startEvent>
    定时日期类型
    timeDate:一次启动，具体到日期
    timeDuration：设置多长时间后启动
    timeCycle：周期性启动，循环执行
    ```

* **异常启动事件**：不能通过API触发，它总是在另外一个流程抛出异常结束事件的时候被触发

  * 类比java中的异常，throw是异常结束，catch是异常启动

  * 圆圈中雷电符号

    ``` xml
    <startEvent id="errorstartevent1" name="error start">
    	<errorEventDefinition errorRef="AIA001" />
    </startEvent>
    ```

* **消息启动事件**：通过一个消息名称触发启动，也可以结合消息抛出事件一起使用

  * 西圆圈中嵌入一个空心的信封图表

    ``` xml
    <message id="resendFile" name="重新发送文件">
    ...
    <startEvent id="messageStart">
    	<messageEventDefinition messageRef="reSendFile" />
    </startEvent>
    ```

> **结束事件**：粗圆圈表示，抛出型()

* **空结束事件**：结束

* **异常结束事件**：粗圆圈里有雷电符号

  * 定义抛出的错误代码，如果找到了异常开始时间定义的异常代码，则触发异常开始事件，类似于钩子
  * 异常事件错误代码不能为空

  ``` xml
  <endEvent id="errorstartevent1" name="error end">
  	<errorEventDefinition errorRef="AIA001" />
  </endEvent>
  ```

* **终止结束事件**：结束整个流程实例。空结束事件中间加一个实心原点

  ``` xml
  <endEvent id="errorstartevent1" name="error end">
  	<terminateEventDefinition />
  </endEvent>
  ```

* **取消结束事件**：可以取消一个事物子流程，空结束事件嵌入一个“X”图形

  ```xml
  <endEvent id="errorstartevent1" name="error end">
  	<cancelEventDefinition />
  </endEvent>
  ```

#### 顺序流

* **标准顺序流**

  ```xml
  <sequenceFlow id="flow" sourceRef="startEvent" targetRef="userTastk" />
  ```

  sourceRef 执行顺序流的源

  targetRef属性执行目的模型

* **条件顺序流**

  标准流上添加条件表达式

  ``` xml
  <sequenceFlow id="flow" sourceRef="startEvent" targetRef="userTastk">
  	<conditionExpression xsi:type="tFormalExpression">
  		<![CDATA[${pass==true}]]>
  	</conditionExpression>
  </sequenceFlow>
  ```

  xsi:type指定表达式类型，目前条件表达式只支持UEL

#### 任务

* **用户任务(userTask)**

  ``humanPerFormer`  `formalExpression` 将任务分配给用户

  `potentialOwner` `formalExpression`  将用户和组混合分配

  * activiti扩展用户任务属性

    * activiti:assigness:指定单个用户任务处理人 代替humanPerFormer

    * activiti:cadidateUsers:指定用户任务多个候选人，多个逗号分开 代替potentialOwner

    * activiti:cadidateGroups:指定候选组，逗号分开,代替potentialOwner

    * activiti:dueDate：设置用户任务到期日

    * activiti:priority:用户任务优先级[0,100]

    * activiti允许任务上添加任务监听

      * 选项有：create创建任务、assignment分配任务、complete完成任务

        ```xml
        <userTask id="..." name="...">
        	<extensionElements>
        	<activiti:taskListener class="..." event="complete">
        	</extensionElements>
        </userTask>
        除了class属性指定文件之外，还可以定义expression和delegateExpression以表达式方式处理
        ```

* **脚本任务(scriptTask) **

  ```xml
  <scriptTask id="..." name="..." scriptFormat="指定脚本任务类型(groovy,javascript,juel)">
  	...
  </scriptTask>
  ```

* **web service任务(serviceTask)**

  * 调用外部的webservice自选，支持标准的webservice和rest分隔的service

* **业务规则任务(businessRuleTask)**

  * 支持jboss规则引擎drools，只要把流程文件和规则引擎文件.drl 一同打包部署系统中，并加入jar即可。

  * 概念：就是把业务收交由规则引擎处理，规则引擎根据各种条件判断计算出最终将结果，最后把结果返回给调用者，调用者为流程引擎

    ```xml
    <businessRuleTask id="..." name="..." activiti:rules="rule1,rule2" activiti:ruleVariablesInput="${message}" 
    	activiti:exclude="false" activiti:resultVariableName="relesOutput">
    </businessRuleTask>
    ```

  * 扩展属性：便于与Drools整合

    * activiti:rules：在drl规则文件中定义的规则，多个逗号分开，空执行全部规则
    * activiti:ruleVariablesInput：业务执行需要的数据源，多个逗号分开
    * activiti:exclude：设置是否排除某些规则，值为false则不排除，值为true则忽略规则
      * 如果activiti:rules为空同时activiti:exclude为false，则不执行任何规则
    * activiti:resultVariableName：规则执行结果变量

* **邮件任务**：(不属于BPMN2.0规范，activiti在serviceTask上扩展任务 activit:type="email")

  * 通过activiti发送邮件，需要配置邮件服务器

* **mule任务**：(不属于BPMN2.0规范,使用sendTask)

* **camel任务**：(不属于BPMN2.0规范,serivceTask扩展的任务模型)

* **手动任务**：(manualTask) 仅仅用来定义bpm不能完成的任务，人工建模

* **java service任务**：(不属于BPMN2.0规范，activiti扩展的java语言的serviceTask)

* **shell任务**：执行本地操作系统中的脚本、命令，基于serviceTask的扩展(activit:type="shell")

* **接收任务(receiveTask)**：功能简单且单一任务，只能通过runtimeService接口的signal方法触发接收任务

* **多实例**

  * 多实例语序业务流程中某一个任务甚至子流程可以重复执行多次。多个实例可以选择顺序执行，还可以选择并行执行多实例任务或子流程
  * 多实例支持的任务类型：用户、脚本、javaservice、webservice、业务规则、邮件、手动任务、接收任务、子流程
  * 多实例的几个属性变量可以通过execution.getVariable()获取
    * nrOfInstances：实例总数
    * nrOfActiveInstances:当前活动为完成的实例数量，对于按照顺序执行的多实例，该值为1
    * nrOfCompletedInstances：已经完成的实例数量
    * loopCounter：多实例运行过程中,for-each循环中当前的索引值

#### 网关

* **排它网关**(exclusive gateway)：用来对流程中的决定进行建模，多个线路计算结果都为true，那么只会执行第一个true的网关，如果多个没有true，那么抛出异常，需要配合条件孙序列配合使用

* **并行网关**(parallel gateway)：对并发的任务进行流程建模，单个任务拆分多个或将多个合并
  * 并行网关功能取决于输入、输出流

* **包容网关**(inclusive gateway)：包含排它网关和并行网关

* **事件网关**(event gateway)

  

#### 子流程与调用活动

* **子流程**：`subProcess`
* **调用活动**：解决流程的通用性，和子流程作用一直，但是表现方式不同，使用一个调用活动取代嵌入式流程方式的活动即可。
* **事件子流程**：事件子流程和子流程类似，把一系列的活动归结到一起处理，不同的是事件子流程不能直接启动，而要被动的由其他事件触发启动。添加了一个 **`triggeredByEvent`** 属性表示此至子流程只能由事件触发后被动启动。
* **事物子流程**：也称为事务块，用来处理一组必须在同一个事务中完成的活动，具有ACID特性。

#### 边界与中间事件

* **边界事件(boundaryEvent)**

  * 是绑定在活动上的 ”捕获型“ 事件，会一直监听所有处于运行中活动的某种事件的触发。一旦触发边界事件，当前的活动就会被终端，然后按照边界事件之后的输出流执行。每个边界事件类型都是通过属性**`attachedToRef`** 指定 “附加” 到抛出边界事件的活动上。

  * 所有的边界事件子类型都需要包含在 **`boundaryEvent` **标签中。`cancelAcitivity` 属性可以取 `true`  和`false` 两个值，用来指定捕获边界事件之后是否取消执行输出流指定的活动。

  * **定时器边界事件**：timerEventDefinition

  * **异常边界事件**：errorEventDefinition

  * **信号边界事件**：signalEventDefinition

  * **取消边界事件**：cancelEventDefinition

  * **补偿边界事件**：compensateEventDefinition

    

* **中间捕获事件(intermediateCatchEvent)**

  * 根据事件的不同类型需要使用不同的方式才能继续执行后续的输出流的活动。
  * 边界事件需要附加在一个活动上，不需要输入流，而中间捕获事件必须连接一个输入流和输出流，所以根据这个特性命名为中间捕获事件。
  * **定时器中间捕获事件 **：timerEventDefinition
  * **信号中间捕获事件**：signalEventDefinition
  *  **消息中间捕获事件**：messageEventDefinition

* **中间抛出事件(intermediateThrowEvent)**

  * **空中间抛出事件**：timerEventDefinition
  * **信号中间抛出事件**：signalEventDefinition

#### 监听器

* 监听器是在规范基础上扩展的功能，通过配置监听器的方法监听各种动作，如流程启动、结束、创建、完成等
* 类型：执行监听器、任务监听器

























































