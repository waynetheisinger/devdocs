---
layout: howtom2devgde_chapters
title: Basics of Building a Service
---

# Basics of Building a Service

<p><a href="https://github.com/magento/devdocs/blob/master/guides/m2devgde/v1.0.0.0/svc-framework/build-svc.md" target="_blank"><em>Help us improve this page</em></a>&nbsp;<img src="{{ site.baseurl }}/common/images/newWindow.gif"/></p>

A _service_ is basically a contract between code that uses the service and an integration that implements the service. The service itself is PHP code--typically one or more interfaces, classes, and methods. To review the properties of a service, see TBD.

The code that uses a service should depend on the interface rather than on the service implementation to enable the use of a different implementation if needed.

## About the Service Interface

Some important characteristics of a service interface:

*	The methods must be annotated to describe the types of input, which are used to generate contracts for the WebAPI framework. 
	
	Input arguments can be scalar types, arrays or objects. If objects are used, they should be implemented as [Service Data Objects](#about-service-data-objects).

*	The metods must be annotated to describe the types of the output, which are used to generate contracts for WebAPI framework. 

	Output argument can be scalar types, arrays or objects. If objects are used, they should be implemented as [Service Data Objects](#about-service-data-objects).

*	Every interface must be versioned and the version must be a suffix of the interface name.

	Versions must be numbered V1, V2, and so on.
	
To see an example of a Customer service interface, see <a href="https://github.com/magento/magento2/blob/master/app/code/Magento/Customer/Service/V1/CustomerAccountServiceInterface.php" target="_blank">app/code/Magento/Customer/Service/V1/CustomerAccountServiceInterface.php</a> on the Magento 2 github.

## About Service Data Objects

A _service data object_ carries data to and from service methods. Service data objects differ from _business objects_ or _data access objects_ in that a service data object only stores and retrieves its own data (accessors and mutators). 

Service data objects define the interface that can be used by PHP clients to understand what data is available from the service. The service data object carries data from the service's response to PHP clients and, in the same way, PHP clients use service data objects to pass data to the service or to exchange data with other clients.

Service data objects are:

*	Simple objects that should not contain any business logic (in other words, service data objects contain no long that requires testing). 

*	Immutable; they have only getters and no business logic. 

*	Constructed using a data object builder. Data is set on the service data objects by the service or by client code.

	If data is set in client code, service data objects can be returned to the client to use the data or can passed to the service to consume the data. 
	
*	Reside in a namespace that reflects the version of service data (same as the service itself).

### Implementing a Service Data Object

General rules for implementing a service data object:

*	The service data object class must reside in the `[extension_namespace]\Service\Data\V[xx]` namespace.

*	A service data object typically has no PHP interface.

*	A service data object must have getters which clearly describes all the data which data object can contain.

*	Annotations in the code must clearly describe the parameters and outputs of all methods.

*	Getters should rely on the `_get` method provided for convenience in the `\Magento\Framework\Service\Data\AbstractObjectBuilder` class.

*	A service data object should extend `\Magento\Framework\Service\Data\AbstractObject` or any of its children. _This class is not currently in the Magento 2 public github repository._

### Sample Service Data Object

Here is a sample service data object:

<script src="https://gist.github.com/xcomSteveJohnson/6193ba94d58b7ee3b7c7.js"></script>

### Sample Data Object Builder

Here is a sample data object builder on which the preceding service data object is constructed:

<script src="https://gist.github.com/xcomSteveJohnson/f7ccaf017ea745b895ec.js"></script>

Note the following:

*	A data object builder should be used to construct the service data object.

*	Setters should rely on the method `_set` provided for convenience in the `\Magento\Framework\Service\Data\AbstractObjectBuilder` class. _This class is not currently in the Magento 2 public github repository._