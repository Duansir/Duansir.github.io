---
layout: post
title: 任务与中断同步
categories: freertos
description: 学习freertos的笔记。
keywords: 二值信号量 事件标志 
---



# 任务与中断同步

## 1、采用二值信号量实现任务与中断同步

Take：加一    Give：减一

API函数：

```c
1、portBASE_TYPE xSemaphoreTake(xSemaphoreHandle xSemaphore, portTickType xTicksToWait);
2、portBASE_TYPE xSemaphoreGiveFromISR( xSemaphoreHandle xSemaphore,
                                       portBASE_TYPE *pxHigherPriorityTaskWoken );
```

延迟处理任务调用xSemaphoreTake()时，等效于带阻塞时间地读取队列，如果队列为空的话任务则进入阻塞态。当事件发生后，ISR 简单地通过调用xSemaphoreGiveFromISR()放置一个令牌(信号量)到队列中，使得队列成为满状态。这也使得延迟处理任务解除阻塞态，开始任务处理。

### Example use of xSemaphoreTake()

```c
SemaphoreHandle_t xSemaphore = NULL;

/* A task that creates a mutex type semaphore. */
void vATask( void * pvParameters )
{
    /* A semaphore is going to be used to guard a shared resource. In this case
    a mutex type semaphore is created because it includes priority inheritance
    functionality. */
    xSemaphore = xSemaphoreCreateMutex();
    
    /* The rest of the task code goes here. */
    for( ;; )
    {
       /* ... */
    }
}

/* A task that uses the mutex. */
void vAnotherTask( void * pvParameters )
{
    for( ;; )
    {
        /* ... Do other things. */
        if( xSemaphore != NULL )
        {
            /* See if the mutex can be obtained. If the mutex is not available
            wait 10 ticks to see if it becomes free. */
            if( xSemaphoreTake( xSemaphore, 10 ) == pdTRUE )
            {
                /* The mutex was successfully obtained so the shared resource can be
                accessed safely. */
                /* ... */
                /* Access to the shared resource is complete, so the mutex is
                returned. */
                xSemaphoreGive( xSemaphore );
            }
            else
            {
                /* The mutex could not be obtained even after waiting 10 ticks, so
                the shared resource cannot be accessed. */
            }
        }
    }
}
```

### Example use of xSemaphoreGiveFromISR

```c
#define LONG_TIME 0xffff
#define TICKS_TO_WAIT 10

SemaphoreHandle_t xSemaphore = NULL;

/* Define a task that performs an action each time an interrupt occurs. The
Interrupt processing is deferred to this task. The task is synchronized with the
interrupt using a semaphore. */
void vATask( void * pvParameters )
{
    /* It is assumed the semaphore has already been created outside of this task. */
    for( ;; )
    {
        /* Wait for the next event. */
        if( xSemaphoreTake( xSemaphore, portMAX_DELAY ) == pdTRUE )
        {
            /* The event has occurred, process it here. */
            ...
            /* Processing is complete, return to wait for the next event. */
        }
    }
}

/* An ISR that defers its processing to a task by using a semaphore to indicate
when events that require processing have occurred. */
void vISR( void * pvParameters )
    {
    BaseType_t xHigherPriorityTaskWoken = pdFALSE;
    
    /* The event has occurred, use the semaphore to unblock the task so the task
    can process the event. */
    xSemaphoreGiveFromISR( xSemaphore, &xHigherPriorityTaskWoken );
    
    /* Clear the interrupt here. */
    /* Now the task has been unblocked a context switch should be performed if
    xHigherPriorityTaskWoken is equal to pdTRUE. NOTE: The syntax required to perform
    a context switch from an ISR varies from port to port, and from compiler to
    compiler. Check the web documentation and examples for the port being used to
    find the syntax required for your application. */
    portYIELD_FROM_ISR( xHigherPriorityTaskWoken );
}
```



[FreeRTOS学习笔记——二值型信号量]: https://blog.csdn.net/xukai871105/article/details/43153177



## 2、采用事件标志实现任务与中断同步

任务间事件标志组的实现是指各个任务之间使用事件标志组实现任务的通信或者同步机制。

API函数

```c
1、EventBits_t xEventGroupWaitBits( const EventGroupHandle_t xEventGroup,
                                    const EventBits_t uxBitsToWaitFor,
                                    const BaseType_t xClearOnExit,
                                    const BaseType_t xWaitForAllBits,
                                    TickType_t xTicksToWait );
2、BaseType_t xEventGroupSetBitsFromISR( EventGroupHandle_t xEventGroup,
                                        const EventBits_t uxBitsToSet,
                                        BaseType_t *pxHigherPriorityTaskWoken );
3、
```



### Example use of xEventGroupSetBitsFromISR()

```c
#define BIT_0 ( 1 << 0 )
#define BIT_4 ( 1 << 4 )

/* An event group which it is assumed has already been created by a call to
xEventGroupCreate(). */
EventGroupHandle_t xEventGroup;

void anInterruptHandler( void )
{
    BaseType_t xHigherPriorityTaskWoken, xResult;
    
    /* xHigherPriorityTaskWoken must be initialized to pdFALSE. */
    xHigherPriorityTaskWoken = pdFALSE;
    
    /* Set bit 0 and bit 4 in xEventGroup. */
    xResult = xEventGroupSetBitsFromISR(xEventGroup, /* The event group being updated. */
                                        BIT_0 | BIT_4 /* The bits being set. */
                                        &xHigherPriorityTaskWoken );
    
    /* Was the message posted successfully? */
    if( xResult != pdFAIL )
    {
        /* If xHigherPriorityTaskWoken is now set to pdTRUE then a context
        switch should be requested. The macro used is port specific and will
        be either portYIELD_FROM_ISR() or portEND_SWITCHING_ISR() - refer to
        the documentation page for the port being used. */
    	portYIELD_FROM_ISR( xHigherPriorityTaskWoken );
    }
}
```

### Example use of xEventGroupWaitBits

```c
#define BIT_0 ( 1 << 0 )
#define BIT_4 ( 1 << 4 )

void aFunction( EventGroupHandle_t xEventGroup )
{
    EventBits_t uxBits;
    const TickType_t xTicksToWait = pdMS_TO_TICKS( 100 );
    
    /* Wait a maximum of 100ms for either bit 0 or bit 4 to be set within
    the event group. Clear the bits before exiting. */
    uxBits = xEventGroupWaitBits(
                xEventGroup, /* The event group being tested. */
                BIT_0 | BIT_4, /* The bits within the event group to wait for. */
                pdTRUE, /* BIT_0 and BIT_4 should be cleared before returning. */
                pdFALSE, /* Don't wait for both bits, either bit will do. */
                xTicksToWait );/* Wait a maximum of 100ms for either bit to be set. */
    
    if( ( uxBits & ( BIT_0 | BIT_4 ) ) == ( BIT_0 | BIT_4 ) )
    {
   	     /* xEventGroupWaitBits() returned because both bits were set. */
    }
    else if( ( uxBits & BIT_0 ) != 0 )
    {
   		 /* xEventGroupWaitBits() returned because just BIT_0 was set. */
    }
    else if( ( uxBits & BIT_4 ) != 0 )
    {
    	/* xEventGroupWaitBits() returned because just BIT_4 was set. */
    }
    else
    {
        /* xEventGroupWaitBits() returned because xTicksToWait ticks passed
        without either BIT_0 or BIT_4 becoming set. */
    }
}
```



[FreeRTOS 事件标志组]: https://www.cnblogs.com/yangguang-it/p/7189607.html

