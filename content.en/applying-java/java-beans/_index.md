---
title: 'Java Beans'
weight: 1
---
##Java Beans

This chapter provides an overview of creating Java Beans. Beans are important because they allow you to build complex systems from software components. These components may be provided by you or supplied by one or more different vendors. Java Beans use an architecture called JavaBeans that specifies how these building blocks can operate together.

To better understand the value of Beans, consider the following. Hardware designers have a wide variety of components that can be integrated together to construct a system. Resistors, capacitors, and inductors are examples of simple building blocks. Integrated circuits provide more advanced functionality. All of these different parts can be reused. It is not necessary or possible to rebuild these capabilities each time a new system is needed. Also, the same pieces can be used in different types of circuits. This is possible because the behavior of these components is understood and documented.

The software industry also sought the benefits of reusability and interoperability of a component-based approach. To realize these benefits, a component architecture is needed that allows programs to be assembled from software building blocks, perhaps provided by different vendors. It must also be possible for a designer to select a component, understand its capabilities, and incorporate it into an application. When a new version of a component becomes available, it should be easy to incorporate this functionality into existing code. JavaBeans provides just such an architecture.

## What Is a Java Bean?

A _Java Bean_ is a software component that has been designed to be reusable in a variety of different environments. There is no restriction on the capability of a Bean. It may perform a simple function, such as obtaining an inventory value, or a complex function, such as forecasting the performance of a stock portfolio. A Bean may be visible to an end user. One example of this is a button on a graphical user interface. A Bean may also be invisible to a user. Software to decode a stream of multimedia information in real time is an example of this type of building block. Finally, a Bean may be designed to work autonomously  

on a user’s workstation or to work in cooperation with a set of other distributed components. Software to generate a pie chart from a set of data points is an example of a Bean that can execute locally. However, a Bean that provides real- time price information from a stock or commodities exchange would need to work in cooperation with other distributed software to obtain its data.

## Advantages of Beans

 The following list enumerates some of the benefits that JavaBeans technology provides for a component developer:

- A Bean obtains all the benefits of Java’s “write-once, run-anywhere” paradigm.

- The properties, events, and methods of a Bean that are exposed to another application can be controlled.

- Auxiliary software can be provided to help configure a Bean. This software is only needed when the design-time parameters for that component are being set. It does not need to be included in the run-time environment.

- The state of a Bean can be saved in persistent storage and restored at a later time.

- A Bean may register to receive events from other objects and can generate events that are sent to other objects.

## Introspection

 At the core of Bean programming is introspection. This is the process of analyzing a Bean to determine its capabilities. This is an essential feature of the JavaBeans API because it allows another application, such as a design tool, to obtain information about a component. Without introspection, the JavaBeans technology could not operate.

There are two ways in which the developer of a Bean can indicate which of its properties, events, and methods should be exposed. With the first method, simple naming conventions are used. These allow the introspection mechanisms to infer information about a Bean. In the second way, an additional class that extends the **BeanInfo** interface is provided that explicitly supplies this information. Both approaches are examined here.  

## Design Patterns for Properties

 A property is a subset of a Bean’s state. The values assigned to the properties determine the behavior and appearance of that component. A property is set through a setter method. A property is obtained by a getter method. There are two types of properties: simple and indexed.

## Simple Properties

 A simple property has a single value. It can be identified by the following design patterns, where **N** is the name of the property and **T** is its type:

public T getN() 
public void setN(T arg)

A read/write property has both of these methods to access its values. A read- only property has only a get method. A write-only property has only a set method.

Here are three read/write simple properties along with their getter and setter methods:  
```
private double depth, height, width;
public double get Depth() { return depth;
} public void setDepth (double d) { depth = d; }
public double getHeight() { return height;
} public void setHeight (double h) { height = h;
}
public double getWidth() { return width;
} public void setWidth (double w) { width = W;
}
```
## NOTE

 For a **boolean** property, a method of the form **is**PropertyName() can also be used as an accessor.

## Indexed Properties

 An indexed property consists of multiple values. It can be identified by the following design patterns, where **N** is the name of the property and **T** is its type:

public T getN(int index); public void setN(int index, T value); public T[ ] getN(); public void setN(T values[ ]);

Here is an indexed property called **data** along with its getter and setter methods:  
```
private double data[ ];
public double get Data (int index) { return data [index];
} public void setData (int index, double value) { data [index] value; =
} public double[] getData() {
return data;
} public void setData (double[] values) { data = new double [values.length]; System.arraycopy (values, 0, data, 0, values.length); 
}
```
## Design Patterns for Events

 Beans use the delegation event model that was discussed earlier in this book. Beans can generate events and send them to other objects. These can be identified by the following design patterns, where **T** is the type of the event:

These methods are used to add or remove a listener for the specified event. The version of **addTListener()** that does not throw an exception can be used to multicast an event, which means that more than one listener can register for the event notification. The version that throws **TooManyListenersException** unicasts the event, which means that the number of listeners can be restricted to one. In either case, **removeTListener()** is used to remove the listener. For example, assuming an event interface type called **TemperatureListener**, a Bean that monitors temperature might supply the following methods:

public void addTemperatureListener(TemperatureListener tl) {

...

}

public void removeTemperatureListener(TemperatureListener tl) {  

...

}

## Methods and Design Patterns

 Design patterns are not used for naming nonproperty methods. The introspection mechanism finds all of the public methods of a Bean. Protected and private methods are not presented.

## Using the BeanInfo Interface

 As the preceding discussion shows, design patterns implicitly determine what information is available to the user of a Bean. The **BeanInfo** interface enables you to explicitly control what information is available. The **BeanInfo** interface defines several methods, including these:

PropertyDescriptor[ ] getPropertyDescriptors() EventSetDescriptor[ ] getEventSetDescriptors() MethodDescriptor[ ] getMethodDescriptors()

They return arrays of objects that provide information about the properties, events, and methods of a Bean. The classes **PropertyDescriptor**, **EventSetDescriptor**, and **MethodDescriptor** are defined within the **java.beans** package, and they describe the indicated elements. By implementing these methods, a developer can designate exactly what is presented to a user, bypassing introspection based on design patterns.

When creating a class that implements **BeanInfo**, you must call that class bnameBeanInfo, where bname is the name of the Bean. For example, if the Bean is called **MyBean**, then the information class must be called **MyBeanBeanInfo**.

To simplify the use of **BeanInfo**, JavaBeans supplies the **SimpleBeanInfo** class. It provides default implementations of the **BeanInfo** interface, including the three methods just shown. You can extend this class and override one or more of the methods to explicitly control what aspects of a Bean are exposed. If you don’t override a method, then design-pattern introspection will be used. For example, if you don’t override **getPropertyDescriptors()**, then design patterns are used to discover a Bean’s properties. You will see **SimpleBeanInfo** in action later in this chapter.

## Bound and Constrained Properties

  

## Bound and Constrained Properties

 A Bean that has a bound property generates an event when the property is changed. The event is of type **PropertyChangeEvent** and is sent to objects that previously registered an interest in receiving such notifications. A class that handles this event must implement the **PropertyChangeListener** interface.

A Bean that has a constrained property generates an event when an attempt is made to change its value. It also generates an event of type **PropertyChangeEvent**. It too is sent to objects that previously registered an interest in receiving such notifications. However, those other objects have the ability to veto the proposed change by throwing a **PropertyVetoException**. This capability allows a Bean to operate differently according to its run-time environment. A class that handles this event must implement the **VetoableChangeListener** interface.

## Persistence

 Persistence is the ability to save the current state of a Bean, including the values of a Bean’s properties and instance variables, to nonvolatile storage and to retrieve them at a later time. The object serialization capabilities provided by the Java class libraries are used to provide persistence for Beans.

The easiest way to serialize a Bean is to have it implement the **java.io.Serializable** interface, which is simply a marker interface. Implementing **java.io.Serializable** makes serialization automatic. Your Bean need take no other action. Automatic serialization can also be inherited. Therefore, if any superclass of a Bean implements **java.io.Serializable**, then automatic serialization is obtained.

When using automatic serialization, you can selectively prevent a field from being saved through the use of the **transient** keyword. Thus, data members of a Bean specified as **transient** will not be serialized.

If a Bean does not implement **java.io.Serializable**, you must provide serialization yourself, such as by implementing **java.io.Externalizable**. Otherwise, containers cannot save the configuration of your component.

## Customizers

A Bean developer can provide a customizer that helps another developer configure the Bean. A customizer can provide a step-by-step guide through the process that must be followed to use the component in a specific context. Online documentation can also be provided. A Bean developer has great flexibility to develop a customizer that can differentiate his or her product in the marketplace.

## The JavaBeans API

 The JavaBeans functionality is provided by a set of classes and interfaces in the **java.beans** package. Beginning with JDK 9, this package is in the **java.desktop** module. This section provides a brief overview of its contents. Table 34-1 lists the interfaces in **java.beans** and provides a brief description of their functionality. Table 34-2 lists the classes in **java.beans**.

| Interface | Description |
|------------|-------------|
| AppletInitializer | Methods in this interface are used to initialize Beans that are also applets. (Deprecated by JDK 9.)|
| BeanInfo | This interface allows a designer to specify information about the properties, events, and methods of a Bean.|
| Customizer | This interface allows a designer to provide a graphical user interface through which a Bean may be configured.|
| DesignMode | Methods in this interface determine if a Bean is executing in design mode. |
| ExceptionListener | A method in this interface is invoked when an exception has occurred. |
| PropertyChangeListener | A method in this interface is invoked when a bound property is changed. |
| PropertyEditor | Objects that implement this interface allow designers to change and display property values. |
| VetoableChangeListener | A method in this interface is invoked when a constrained property is changed. |
| Visibility | Methods in this interface allow a Bean to execute in environments where a graphical user interface is not available. |

**Table 34-1** The Interfaces in **java.beans**  

| Class | Description |
|----------| --------- |
|BeanDescriptor|This class provides information about a Bean. It also allows you to associate a customizer with a Bean.|
|Beans|This class is used to obtain information about a Bean.|
|Default PersistenceDelegate|A concrete subclass of PersistenceDelegate.|
|Encoder|Encodes the state of a set of Beans. Can be used to write this information to a stream.|
|EventHandler|Supports dynamic event listener creation.|
|EventSet Descriptor|Instances of this class describe an event that can be generated by a Bean.|
|Expression|Encapsulates a call to a method that returns a result.|
|FeatureDescriptor|This is the superclass of the PropertyDescriptor, EventSet Descriptor, and MethodDescriptor classes, among others.|
|Indexed PropertyChangeEvent|A subclass of PropertyChangeEvent that represents a change to an indexed property.|
|Indexed PropertyDescriptor|Instances of this class describe an indexed property of a Bean.|
|IntrospectionException|An exception of this type is generated if a problem occurs when analyzing a Bean.|
|Introspector|This class analyzes a Bean and constructs a BeanInfo object that describes the component.|
|MethodDescriptor|Instances of this class describe a method of a Bean.|
|ParameterDescriptor|Instances of this class describe a method parameter.|
|PersistenceDelegate|Handles the state information of an object.|
|PropertyChangeEvent|This event is generated when bound or constrained properties are changed. It is sent to objects that registered an interest in these events and that implement either the PropertyChangeListener or VetoableChangeListener interfaces.|
|PropertyChangeListener Proxy|Extends EventListenerProxy and implements PropertyChangeListener.|
|PropertyChangeSupport|Beans that support bound properties can use this class to notify   PropertyChangeListener objects|
|Property Descriptor|Instances of this class describe a property of a Bean.|
|PropertyEditor Manager|This class locates a PropertyEditor object for a given type.|
|PropertyEditorSupport|This class provides functionality that can be used when writing PropertyVetoException|
|property editors.|An exception of this type is generated if a change to a constrained property is vetoed.|
|SimpleBeanInfo|This class provides functionality that can be used when writing BeanInfo classes.|
|Statement|Encapsulates a call to a method.|
|VetoableChangeListener Proxy|Extends EventListenerProxy and implements VetoableChangeListener.|
|VetoableChangeSupport|Beans that support constrained properties can use this class to notify VetoableChangeListener objects.|
|XMLDecoder|Used to read a Bean from an XML document.|
|XMLEncoder|Used to write a Bean to an XML document.|


**Table 34-2** The Classes in **java.beans**

Although it is beyond the scope of this chapter to discuss all of the classes, four are of particular interest: **Introspector**, **PropertyDescriptor**,  

## EventSetDescriptor
**EventSetDescriptor**, and **MethodDescriptor**. Each is briefly examined here.

## Introspector

 The **Introspector** class provides several static methods that support introspection. Of most interest is **getBeanInfo()**. This method returns a **BeanInfo** object that can be used to obtain information about the Bean. The **getBeanInfo()** method has several forms, including the one shown here:

static BeanInfo getBeanInfo(Class<?> bean) throws IntrospectionException

The returned object contains information about the Bean specified by _bean._

## PropertyDescriptor

 The **PropertyDescriptor** class describes the characteristics of a Bean property. It supports several methods that manage and describe properties. For example, you can determine if a property is bound by calling **isBound()**. To determine if a property is constrained, call **isConstrained()**. You can obtain the name of a property by calling **getName()**.

## EventSetDescriptor

 The **EventSetDescriptor** class represents a set of Bean events. It supports several methods that obtain the methods that a Bean uses to add or remove event listeners, and to otherwise manage events. For example, to obtain the method used to add listeners, call **getAddListenerMethod()**. To obtain the method used to remove listeners, call **getRemoveListenerMethod()**. To obtain the type of a listener, call **getListenerType()**. You can obtain the name of an event set by calling **getName()**.

## MethodDescriptor

 The **MethodDescriptor** class represents a Bean method. To obtain the name of the method, call **getName()**. You can obtain information about the method by calling **getMethod()**, shown here:

Method getMethod()

An object of type **Method** that describes the method is returned.  

## A Bean Example

 This chapter concludes with an example that illustrates various aspects of Bean programming, including introspection and using a **BeanInfo** class. It also makes use of the **Introspector**, **PropertyDescriptor**, and **EventSetDescriptor** classes. The example uses three classes. The first is a Bean called **Colors**, shown here:  
```js
 // A simple Bean.
import java.awt.*;
import java.awt.event.*;
import java.io.Serializable;
public class Colors extends Canvas implements Serializable { transient private Color color; // not persistent private boolean rectangular; // is persistent
public Colors () {
addMouseListener(new MouseAdapter() { public void mouse Pressed (MouseEvent me) { change ();
} });
rectangular = false; setSize (200, 100);
}
change();
public boolean getRectangular () { return rectangular; }
public void setRectangular (boolean flag) { this.rectangular flag;
}
repaint ();
public void change() { color = randomColor();
repaint();
}
private Color randomColor() {
int r = (int) (255* Math.random()); int g = (int) (255* Math.random());
int b = (int) (255*Math.random()); return new Color (r, g, b);
}
public void paint (Graphics g) {
int hd.height; int w =d.width;
Dimension d = getSize(); if (rectangular) { }
g.setColor (color);
g.fillRect(0, 0, w-1, h-1);
else {
g.fillOval (0, 0, w-1, h-1);
}
}
}
```

The **Colors** Bean displays a colored object within a frame. The color of the component is determined by the private **Color** variable **color**, and its shape is determined by the private **boolean** variable **rectangular**. The constructor defines an anonymous inner class that extends **MouseAdapter** and overrides its **mousePressed()** method. The **change()** method is invoked in response to mouse presses. It selects a random color and then repaints the component. The **getRectangular()** and **setRectangular()** methods provide access to the one property of this Bean. The **change()** method calls **randomColor()** to choose a color and then calls **repaint()** to make the change visible. Notice that the **paint()** method uses the **rectangular** and **color** variables to determine how to present the Bean.

The next class is **ColorsBeanInfo**. It is a subclass of **SimpleBeanInfo** that provides explicit information about **Colors**. It overrides **getPropertyDescriptors()** in order to designate which properties are presented to a Bean user. In this case, the only property exposed is **rectangular**. The method creates and returns a **PropertyDescriptor** object for the **rectangular** property. The **PropertyDescriptor** constructor that is used is shown here:

PropertyDescriptor(String property, Class<?> beanCls) throws IntrospectionException

Here, the first argument is the name of the property, and the second argument is the class of the Bean.  

```js
// A Bean information class. import java.beans.*;
public class Colors BeanInfo extends SimpleBeanInfo { public PropertyDescriptor [] get PropertyDescriptors () { try {
PropertyDescriptor rectangular = new PropertyDescriptor ("rectangular", Colors.class); PropertyDescriptor pd[] = { rectangular}; return pd;
} catch (Exception e) { System.out.println("Exception caught. " + e);
}
return null;
}
}
```

The final class is called **IntrospectorDemo**. It uses introspection to display the properties and events that are available within the **Colors** Bean.  

```js
// Show properties and events. import java.awt.*;
import java.beans.*;
public class IntrospectorDemo { public static void main(String args[]) {
try {
Class<?> c = Class.forName("Colors"); BeanInfo beanInfo Introspector.getBeanInfo (c);
System.out.println("Properties: ");
PropertyDescriptor propertyDescriptor[]
bean Info.getPropertyDescriptors(); for (int i = 0; i < propertyDescriptor.length; i++) { System.out.println("\t" + propertyDescriptor [i].getName());
}
System.out.println("Events: ");
beanInfo.getEvent Set Descriptors();
Event Set Descriptor event Set Descriptor[] = for (int i = 0; i < event Set Descriptor.length; i++) { System.out.println("\t" + event Set Descriptor [i].getName()); }
}
catch (Exception e) { System.out.println("Exception caught." + e);
}
}
}
```

The output from this program is the following:
```js
Properties:
rectangular
Events:
mouseWheel
mouse
mouseMotion
component
hierarchyBounds
focus
hierarchy
propertyChange
inputMethod
key
```

Notice two things in the output. First, because **ColorsBeanInfo** overrides **getPropertyDescriptors()** such that the only property returned is **rectangular**, only the **rectangular** property is displayed. However, because **getEventSetDescriptors()** is not overridden by **ColorsBeanInfo**, design- pattern introspection is used, and all events are found, including those in **Colors**’ superclass, **Canvas**. Remember, if you don’t override one of the “get” methods defined by **SimpleBeanInfo**, then the default, design-pattern introspection is used. To observe the difference that **ColorsBeanInfo** makes, erase its class file and then run **IntrospectorDemo** again. This time it will report more properties.  




