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
	void ExecuteFunction(FString FunctionToCall);
{% endhighlight %}

.cpp
{% highlight cpp %}
void ABasePlayer::ReflectionSystem(FString FunctionToCall)
{
	UFunction* theFunction = FindFunctionChecked(*FunctionToCall);
	if( theFunction )
	{
		void* local = nullptr;
		FFrame* frame = new FFrame(this, theFunction, local);
		CallFunction(*frame, local, theFunction);
		delete frame;
	}
}
{% endhighlight %}