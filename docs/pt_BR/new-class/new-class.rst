Criando Classe SDK
===================

.. _Estrutura Classe SDK:

Estrutura Classe SDK
~~~~~~~~~~~~~~~~~~~~

O módulo de SDK poderá ser expandido a outros sensores (recursos), para isto basta seguir o modelo de
estrutura abaixo, para garantir a integração junto ao sistema da Automação IOT.

**Classe Hipotética**

Este recurso (hipoteticamente) lê dados de uma determinada GPIO e envia para a Base de Dados IoT::

  LeGPIO.py


	from Device import Device
	from Resource import Resource


	class LeGPIO(Resource):

		def __init__(self, *args):

			self.refresh = 1000
			self.pin = None
			self.state = None

			if len(args) == 3:
				self.id = args[0]
				self.gpio = args[1]
			elif len(args) == 4:
				self.id = args[0]
				self.gpio = args[1]
				self.refresh = args[3]

			Device.addResource(self,self)
			self.tsLeGPIOBegin = Device.getTime(self)

		@staticmethod
		def start(cls, protocol, url, keyPublic, keySecret,debug):
			try:
				if(Resource.resourceTime(Device.getTime(cls),cls.tsLeGPIOBegin,cls.refresh)):					
					cls.actionStart(protocol, url, keyPublic, keySecret, debug)
					cls.tsLeGPIOBegin = Device.getTime(cls)
			except:
				pass
			return

		def actionStart(self,protocol, url, keyPublic, keySecret,debug):
			try:
				self.pin = Pin(self.gpio, Pin.IN)
				valuePin = self.pin.value()

                value = Resource.createFeed(self.id, protocol, url, keyPublic, keySecret,valuePin, debug)
			except:
				pass
			return