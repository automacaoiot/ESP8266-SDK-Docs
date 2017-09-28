Exemplos SDK-ESP8266
=====================

.. _Modo de Configuração Manual:

Modo de Configuração Manual
~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
*Recurso Beep* ::


	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "device.h"
	#include "beep.h" 

	void _Device();
	void _Resource();

	DEVICE *Device;
	BEEP *Beep;
       
	void _Device()
	{	
		Device->start();        
		_Resource();
	}

	void _Resource()
	{            
		Beep->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{	
		debugf("Connected");	
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		Device = new DEVICE("SECRET_KEY","PUBLIC_KEY");
		Beep = new BEEP(IDbeep,GPIOXX);

		Device->setNetworkConfig("SSID","PWD");
	}  


*Recurso Dht* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "device.h"
	#include "dht.h"

	DEVICE *Device;
	DHT *Dht;

	void _Device();
	void _Resource();

	void _Device()
	{	
		Device->start();        
		_Resource();
	}

	void _Resource()
	{            
		// DHT    
		Dht->setFilter(false);
		Dht->start();             
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{
		Device->_ledRGB->color(GREEN);
		debugf("Connected");	
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{    
		Device = new DEVICE("SECRET_KEY","PUBLIC_KEY",);		
		Dht=new DHT(idTemperature,idHumidity,GPIOXX, DHTXX);
    
		Device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_DISABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		Device->setNetworkConfig("SSID","PWD");  
	}  

	
*Recurso Flame* ::	

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "device.h"
	#include "flame.h"


	void _Device();
	void _Resource();

	DEVICE *Device;
	FLAME *Flame;
         
	void _Device()
	{	
		Device->start();        
		_Resource();
	}

	void _Resource()
	{        
		Flame->setFilter(true);
		Flame->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{	
		debugf("Connected");	
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		Device = new DEVICE("SECRET_KEY","PUBLIC_KEY",);
		Flame = new FLAME(IDflame,GPIOXX);
	
		Device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_DISABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		Device->setNetworkConfig("SSID","PWD");  
	}  

*Recurso Rele* ::	
	
	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "device.h"
	#include "rele.h"

	DEVICE *Device;
	RELE *Rele;

	void _Device();
	void _Resource();

	void _Device()
	{	
		Device->start();
		_Resource();
	}

	void _Resource()
	{            
		Rele->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{	
		debugf("Connected");	
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		Device = new DEVICE("SECRET_KEY","PUBLIC_KEY",);
		Rele= new RELE(IDrele,GPIOxx);
           
		Device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_DISABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		Device->setNetworkConfig("SSID","PWD");  
	}  	

.. _Modo de Configuração WIFI:

Modo de Configuração WIFI
~~~~~~~~~~~~~~~~~~~~~~~~~
	
*Recurso Rele com Led RGB* ::

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "device.h"
	#include "rele.h"

	DEVICE *Device;
	RELE *Rele;

	void _Device();
	void _Resource();

	void _Device()
	{	
		Device->start();
		_Resource();
	}

	void _Resource()
	{            
		Rele->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{	
		debugf("Connected");
		Device->_ledRGB->color(GREEN);	
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{    
		Device=new DEVICE();   
		Rele= new RELE(GPIOxx);    
    
		//Set GPIO LED RGB
		Device->_ledRGB->set(GPIOxx,GPIOxx,GPIOxx,CATODO);
		Device->_ledRGB->color(RED);
    
		Device->setConfig(SYS_CPU_160MHZ,NONE_SLEEP_T,SYS_FILES_ENABLED,DEBUG_ENABLED,COMMAND_DISABLED);
		Device->setNetworkConfig();        
	}  
	
*Recurso WaterLevel - Com Led RGB e Reset GPIO* ::	

	#include <user_config.h>
	#include <SmingCore/SmingCore.h>
	#include "device.h"
	#include "waterlevel.h"

	void _Device();
	void _Resource();

	//CLASSES
	DEVICE *Device; 
	WATERLEVEL *WaterLevel; 
         
	void _Device()
	{	    
		// Ativa Device
		Device->start();                
	_Resource();
	}

	void _Resource()
	{        
    
		//WATER LEVEL
		WaterLevel->setFilter(true);
		WaterLevel->start();
	}

	//Station connected
	void connected(IPAddress ip, IPAddress mask, IPAddress gateway)
	{	
		Device->ledRGB->color(GREEN);
		debugf("Connected");	
		_Device();
	}

	// Connection failed
	void connectionFailed(String ssid, uint8_t ssidLength, uint8_t *bssid, uint8_t reason)
	{
		debugf("Not Connected");
		system_restart();
	}

	void init()
	{
		Device = new DEVICE();    
		WaterLevel = new WATERLEVEL(GPIO4,);
    
		//Set GPIO LED RGB
		Device->ledRGB->set(GPIO13,GPIO12,GPIO14,CATODO);
		Device->ledRGB->color(RED);
           
		Device->setNetworkConfig();
    
		//Config GPIO to restart device
		Device->setReset(GPIO2);        
	}  		