---
layout: post
title:  "Unreal Engine 4 - Reflection System"
date:   2019-01-28 14:00:00 +0700
categories: [Unreal-CPP]
---
Reflection is the ability of a program to inspect its own code (or other associated code) and change at runtime. By default, C++ isn’t capable of that, however Unreal Engine is kind enough and created this ability!

.h
{% highlight cpp %}
public:
	UFUNCTION(BlueprintCallable, Category = Reflection)
	void ReflectionSystem(FString FunctionToCall);
{% endhighlight %}

.cpp
{% highlight cpp %}
void AMyPlayer::ReflectionSystem(FString FunctionToCall)
{
	/* FindFunctionChecked will return a pointer to a UFunction based on a given FName.
	We use * before our arg to convert our FString to FName */
	UFunction* theFunction = FindFunctionChecked(*FunctionToCall);
	/* For security we check if it's valid */
	if( theFunction )
	{
		/* This pointer is a void pointer, that means it can point to
		anything from raw memory to all types. */
		void* local = nullptr;
		/* We reserve a chunk of memory in the execution stack */
		FFrame* frame = new FFrame(this, theFunction, local);
		/* Call of our function */
		CallFunction(*frame, local, theFunction);
		/* Clean our pointer to avoid memory leak */
		delete frame;
	}
}
{% endhighlight %}

This will create a node, where you will be able to call any function by their name.
Right now I'm studying on how to pass arguments and avoid any type of crash with you wrote a wrong name of function.