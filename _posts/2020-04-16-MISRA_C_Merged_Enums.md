---
layout: post
title: MISRA C Merged Enums
excerpt: "Examples to comply with MISRA C 2012 regarding inappropriate enum cast"
modified: 4/16/2020, 9:00:24
tags: [MISRA, C]
comments: true
category: blog
---

#Introduction

This article suggests being **aware to not merge enums definitions into a principal enum as they usually need an inappropriate cast to be used**. To give an example of how this causes problems let’s suppose two functions are defined to get the functionality for a specific entity called “joint”. These functions use three enum types, one called *E_JOINT_FUNCTION* which identifies the functions that a Wheel Joint type can perform; the other two enums are types for the Upper Joint and the Lower Joint available in the system:

```c
    typedef enum
    {
        NORMAL_FUNCTION,
        DEPORTIVE_FUNCTION,
        HEAVY_DUTY_FUNCTION,
        OFF_ROAD_FUNCTION,
        MAX_FUNCTIONS
    }E_JOINT_FUNCTION;

    typedef enum
    {
        FRONT_UPPER_JOINT_LEFT,
        REAR_UPPER_JOINT_RIGHT,
        FRONT_UPPER_JOINT_LEFT,
        REAR_UPPER_JOINT_RIGHT,
    } E_UPPER_JOINT;

    typedef enum
    {
        LOWER_JOINT_LEFT,
        LOWER_JOINT_RIGHT,
    }E_LOWER_JOINT;

```

The described two functions return the functionality that is being used in the specific Upper Joint or Lower Joint.

```c

    E_JOINT_FUNCTION Get_UJ_to_feature(E_UPPER_JOINT)
    {
        /**/
    }

    E_JOINT_FUNCTION Get_LJ_to_feature(E_LOWER_JOINT)
    {
        /**/
    }
```

In this example, I am going to use these functions to determine the functionality in the Joint that is reporting a failure. There might be different errors in the low-level systems that handle the index of the errors differently from the indexes of Upper Joint and Lower Joint, therefore we create a new type called *E_ERR_JOINT_TYPE*:

```c

    typedef enum /*The order depends of the system features, for example it can be ports in the uC*/
    {
        E_ERR_LJ_LEFT,
        E_ERR_LJ_RIGHT,
        E_ERR_UJ_FRONT_LEFT,
        E_ERR_UJ_REAR_LEFT,
        E_ERR_UJ_FRONT_RIGHT,
        E_ERR_UJ_REAR_RIGHT,
        E_ERR_ADAPTOR,
        E_ERR_MAX
    } E_ERR_JOINT_TYPE;
    
```

Next, we might have some requirements that tell us that we need to report in which part of the joint was the failure present, to do so, we created a new function called *publishErrorLog()*:

    static void publishErrorLog(E_ERR_JOINT_TYPE error);

This function requires the distinction between *E_UPPER_JOINT* and *E_LOWER_JOINT* as they will be processed differently in further processes inside the function. Since there might be different constraints, we decided to maintain the logic of both types of joints on this function. Moreover, to get the feature of the specific joint feature we need to cast from *E_ERR_JOINT_TYPE* to either *E_UPPER_JOINT* or *E_LOWER_JOINT*. This approach yields a problem because, both *E_HSC_CHANNEL* and *E_LSC_CHANNEL* have fewer elements than *E_ERR_CHANNEL_TYPE*, therefore it is possible to pass a value *E_ERR_JOINT_TYPE* that cannot be represented on *E_UPPER_JOINT* or *E_LOWER_JOINT*.

The MISRA Rule 10.3 is related to this previous issue: **Expression assigned to a narrower or different essential type is forbidden**. This problem can be displayed in a thoughtless implementation of *publishErrorLog()*:

```c

    static void publishErrorLog(E_ERR_JOINT_TYPE error)
    {
        E_ERR_JOINT_TYPE channelFunc = MAX_FUNCTIONS;

        /*...*/

        /*UJ errors verification*/
        if((E_ERR_UJ_FRONT_LEFT == error) || (E_ERR_UJ_REAR_LEFT == error) || (E_ERR_UJ_FRONT_RIGHT == error) || (E_ERR_UJ_REAR_RIGHT == error))
        {
            /*Upps this line causes MISRA Rule 10.3 warning- No casting to narrower types*/
            channelFunc = Get_UJ_to_feature((E_UPPER_JOINT) error);
            /*...*/
        }
        /*LJ errors verification*/
        else
        {
            /*Upps this line causes MISRA Rule 10.3 warning- No casting to narrower types*/
            channelFunc = Get_LSC_to_feature((E_LOWER_JOINT) error);
            /*...*/
        }
        /*...*/
    }

```

One approach to remove the MISRA warning is to **split *E_ERR_JOINT_TYPE* into two local arrays that return the specific UJ/LJ**. Those arrays can be placed as parameters in the *Get_UJ_to_feature()* and *Get_LJJ_to_feature()* functions. Notice that the array elements that cannot have an association with the enum type are set with an invalid value, for example, from *E_ERR_JOINT_TYPE* enum, the UJ elements are the third, fourth, fifth, and sixth element, therefore the other elements are set as *E_ERR_MAX*.

```c

    static void publishErrorLog(E_ERR_JOINT_TYPE error)
    {

        static const E_UJ_CHANNEL Translate_Error_to_HSC[E_ERR_MAX]
                                                        = { E_ERR_MAX,  E_ERR_MAX,  E_ERR_UJ_FRONT_LEFT, E_ERR_UJ_REAR_LEFT,
                                                            E_ERR_UJ_FRONT_RIGHT, E_ERR_UJ_REAR_RIGHT, E_ERR_MAX,  E_ERR_MAX};

        static const E_LSC_CHANNEL Translate_Error_to_LSC[E_ERR_MAX]
                                                        = { E_ERR_LJ_LEFT, E_ERR_LJ_RIGHT, E_ERR_MAX, E_ERR_MAX,
                                                            E_ERR_MAX,  E_ERR_MAX,  E_ERR_MAX, E_ERR_MAX};

        E_JOINT_FUNCTION JointFunc = MAX_FUNCTIONS;
        /*...*/

        /*UJ errors verification*/
        if((E_ERR_UJ_FRONT_LEFT == error) || (E_ERR_UJ_REAR_LEFT == error) || (E_ERR_UJ_FRONT_RIGHT == error) || (E_ERR_UJ_REAR_RIGHT == error))
        {
            /*OK- No casting to narrower type*/
            JointFunc = Get_UJ_to_feature(Translate_Error_to_UJ[error]);
            /*...*/
        }
        /*LJ errors verification*/
        else
        {
            /*OK- No casting to narrower type*/
            JointFunc = Get_LJ_to_feature(Translate_Error_to_LJ[error]);
            /*...*/
        }

        /*...*/
    }
```

In this way, we had removed the MISRA warning of the previous example.

