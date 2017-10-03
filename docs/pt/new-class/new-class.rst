Criando Classe SDK
===================
	
.. _Estrutura Classe SDK ESP8266:

Estrutura Classe SDK ESP8266
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

O módulo de SDK ESP8266 poderá ser expandido a outros sensores (recursos), para isto basta seguir o modelo de 
estrutura abaixo, para garantir a integração junto ao sistema da Automação IOT. 

**Classe Hipotética Recurso**

Este recurso (hipoteticamente) lê dados da pressão atmosférica. ::

    Recurso.h
	
	#ifndef INCLUDE_RECURSO_H_
	#define INCLUDE_RECURSO_H_

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "base_device.h"
	

	/**
	* Class RECURSO Set up and executing the resource
	*
	*/
	class Recurso : public BaseDevice
	{
		private:
            
			uint16_t pressureAir;
			
			#ifdef WIFI
				static int _counterRecurso;            

				/**
				* Set the resource over the WIFI network            
				* @param gpio GPIO associated with the resource
				* @param refresh Resource timer update in milliseconds
				*/
				void set(GPIO gpio, uint16_t refresh=1000);                
			#endif
                            
			/**
			 * Perform resource action after receiving callback
			*/
			virtual void _action_Callback();
			/**
			* set mode Config Network 
			* WIFI or Normal
			*/
			virtual void setModeConfigNetwork();
			/**
			* Resource timer will run this rotutine
			*/            
			virtual void _temp_Start();	
			/**
			* Set the resource manually
			* @param id Resource
			* @param gpio Associated with the resource
			* @param refresh Resource timer update in milliseconds
			*/                
			void set(unsigned long id, GPIO gpio, uint16_t refresh=1000);
                        
		protected:		
		
		public:

			Recurso(unsigned long id, GPIO gpio, uint16_t refresh=1000);
			Recurso(GPIO gpio, uint16_t refresh=1000);
			virtual ~Recurso(){};
                
			/**
			* Reads JSON received from IOT database through API
			* @return RAW_DATA
			*/
			uint16_t read();            
	};
	#endif
	
**Construtores**

O número de construtores declarados em nosso recurso, é diretamente proporcional as formas de chamadas que poderemos realizar para inicializar o mesmo. 
Recomendamos a implementação de pelo menos 2 construtores, construtor para inicialização em modo *Normal* e em modo *WIFI*. 
Exemplo:

Modo Normal: ::	

	Recurso::Recurso(unsigned long id, GPIO gpio,uint16_t refresh)
	{
		this->id = id;
		this->gpio = gpio;
		refresh = refresh;
		resourceWifiMode = false;    
	}

Modo WIFI: ::

	Recurso::Recurso(GPIO gpio, uint16_t refresh)
	{
		recursoJson = "Recurso"+String(counterRecurso++);
		resourcesJson[indiceJson++] = recursoJson;
		counterJson = counterRecurso;
		this->gpio = gpio;
		refresh = refresh;
		resourceWifiMode = true;
	}

.. important:: Construtor Recurso Modo WIFI. Deverá sempre conter estas linhas com as devidas modificações.
			   
	- recursoJson = "Recurso"+String(counterRecurso++);
	- resourcesJson[indiceJson++] = recursoJson;
	- counterJson = counterRecurso;

    **recursoJson** deverá conter o nome do recurso e o contador global para identificar sua posição no array resourcesJson. **counterJson** deverá ser atualizado com o valor +1 do contador do recurso em uso.
	
		
**Métodos Virtuais**

*setModeConfigNetwork()*

Método responsável por verificar se o método *set* do recurso, será chamado em modo manual ou WIFI, deveremos também associar a variável *name* ao nome do recurso em uso.
No modo normal o método *selectModeNetwork()* terá o valor 1, no modo WIFI terá o valor 2.  Exemplo ::

	name = "Recurso";
	if(selectModeNetwork()==1) set(this->gpio, refresh); else if(selectModeNetwork()==2) set(this->id, refresh);     


*actionCallback()*

Método responsável por manipular e/ou ler os dados retornados do recurso e pela chamada do método *refreshTimer.start()* responsável por executar o método *actionStart()*.
Exemplo: ::

	read();
	refreshTimer.start();

*actionStart()*

Método responsável por executar acesso ao recurso temporizado, através do método *sendHTTP()*. O tempo de execução deste método é diretamente vinculado ao parâmetro *refresh* (expressa em milisegundos) contida no método *set()*, que possui valor default de 1000 milisegundos.
Exemplo: ::

	sendHTTP(json->parseJson(this->pressureAir));

**Métodos**

*set()*

Método responsável pela configuração do recurso. Configuração que poderá ser realizada através do modo *Manual* ou modo *WIFI* através do construtor correspondente.
Exemplos: 

Modo Manual ::

	set(unsigned long id, GPIO gpio, uint16_t refresh=1000);
	
Modo WIFI ::

	set(GPIO gpio, uint16_t refresh=1000);

*read()*

Método responsável por realizar a leitura dos dados recebidos do recurso.
Exemplo: ::

	this->pressureAir = digitalRead(this->gpio);

