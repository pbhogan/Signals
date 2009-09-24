# Signals

## Overview

A lightweight "signals and slots" implementation using fast delegates.

When GUI programming in C++, delegates and the signals and slots paradigm can vastly simplify your code. It implements the Observer pattern while avoiding all the boilerplate code. I needed a lightweight and efficient implementation that I could just drop into my projects and use without adding weird macros, inheriting from crazy templates or having external dependencies. I wanted something simpler and more efficient than libsigc++, sigslot, and boost.signals.

So I was overjoyed when I discovered a very novel and efficient implementation of delegates in C++ and I realized I could make use of these to create a signals and slots implementation that would be very non-intrusive.

## Features

* Extremely efficient (based on delegates that generate only two lines of ASM code).
* No inheritance required.
* No third-party preprocessor required.
* Works seamlessly with global functions, object methods, virtual methods and static class methods.
* Very portable. This should work on any reasonable C++ compiler.
* Really simple to drop in and use.
* Implemented fully in templates.

## Requires

Signals.h does require STL for std::set, although this could probably be replaced with a custom set class fairly easily if you have one on hand. :)

## Examples

See Example.cpp for more detailed examples. Here is a very simple one:

	// Using Delegate.h

	void MyFunc( int x )
	{
		printf( "MyFunc( %d )", x );
	}


	// Using Signal.h

	class Button
	{	
	public:
		Signal2< int, float > updateLabel;

		void Click( void )
		{
			updateLabel( 2, 34.5 );
		}
	};

	class Label
	{
	public:
		virtual void Update( int i, float f )
		{
			printf( "Update( %d, %.1f )", i, f );
		}
	};

	int main()
	{
		Delegate1< int > delegate;
		delegate.Bind( & MyFunc );
		delegate( 5 );

		Button myButton;
		Label myLabel1;
		Label myLabel2;
		myButton.updateLabel.Connect( & myLabel1, & Label::Update );
		myButton.updateLabel.Connect( & myLabel2, & Label::Update );
		myButton.Click();

		return 0;
	}

## Meta

Written by Patrick Hogan

This project includes a modified version of FastDelegates by Don Clugston. Please see Delegates.h for more information on FastDelegates.

Signals.h is released under the MIT License: http://www.opensource.org/licenses/mit-license.php  
Delegates.h is released into the public domain by the author and may be used for any purpose.

[http://github.com/pbhogan](http://github.com/pbhogan)  
[http://www.gallantgames.com](http://www.gallantgames.com)

