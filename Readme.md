Señales
Visión general
Una implementación ligera de "señales y ranuras" que utiliza delegados rápidos.

Cuando se programa GUI en C ++, los delegados y el paradigma de señales y ranuras pueden simplificar enormemente su código. Implementa el patrón Observer evitando todo el código repetitivo. Necesitaba una implementación liviana y eficiente que pudiera colocar en mis proyectos y usar sin agregar macros extrañas, heredar de plantillas locas o tener dependencias externas. Quería algo más simple y eficiente que libsigc ++, sigslot y boost.signals.

Así que me alegré mucho cuando descubrí una implementación muy novedosa y eficiente de delegados en C ++ y me di cuenta de que podía hacer uso de estos para crear una implementación de señales y ranuras que sería muy no intrusiva.

Características
Extremadamente eficiente (basado en delegados que generan solo dos líneas de código ASM).
No se requiere herencia.
No se requiere preprocesador de terceros.
Funciona a la perfección con funciones globales, métodos de objetos, métodos virtuales y métodos de clases estáticas.
Muy portátil. Esto debería funcionar en cualquier compilador C ++ razonable.
Realmente fácil de colocar y usar.
Implementado íntegramente en plantillas.
Requiere
Signals.h requiere STL para std :: set, aunque esto probablemente podría ser reemplazado con una clase de conjunto personalizado con bastante facilidad si tiene una a mano. :)

Ejemplos
Consulte Example.cpp para obtener ejemplos más detallados. Aquí hay uno muy simple:

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
Meta
Escrito por Patrick Hogan

Este proyecto incluye una versión modificada de FastDelegates de Don Clugston. Consulte Delegates.h para obtener más información sobre FastDelegates.

Signals.h está publicado bajo la licencia MIT: http://www.opensource.org/licenses/mit-license.php
Delegates.h está liberado al dominio público por el autor y puede usarse para cualquier propósito.

http://github.com/pbhogan
http://www.gallantgames.com
