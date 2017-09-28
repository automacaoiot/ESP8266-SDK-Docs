Criando Classe SDK
==================
	
.. _Estrutura Classe SDK ESP8266:

Estrutura Classe SDK ESP8266
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

O módulo de SDK ESP8266 poderá ser expandido a outros sensores (recursos), para isto basta seguir o modelo de 
estrutura abaixo, para garantir a integração junto ao sistema da Automação IOT. 

Classe Hipotética Recurso ::

	#ifndef INCLUDE_RECURSO_H_
	#define INCLUDE_RECURSO_H_

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "base_device.h"

	/**
	* Class RECURSO Set up and executing the resource
	*
	*/
	class RECURSO : public BASE_DEVICE
	{
		private:
             
			#ifdef WIFI
				static int _counterRECURSO;            

				/**
				* Set the resource over the WIFI network            
				* @param gpio GPIO associated with the resource
				* @param refresh Resource timer update in milliseconds
				*/
				void set(GPIO gpio, ... , uint16_t refresh=1000);                
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
			void set(unsigned long id, GPIO gpio, ... ,uint16_t refresh=1000);
                        
		protected:		
		
		public:

			RECURSO(unsigned long id, GPIO gpio, ..., uint16_t refresh=1000);
			RECURSO(GPIO gpio, ... , uint16_t refresh=1000);
			virtual ~RECURSO(){};
                
			/**
			* Reads JSON received from IOT database through API
			* @return RAW_DATA
			*/
			uint16_t read();            
	};
	#endif