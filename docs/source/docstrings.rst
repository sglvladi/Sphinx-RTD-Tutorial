Writing docstrings
==================

There are several different docstring formats which one can use in order to enable Sphinx's ``autodoc`` extension to automatically generate documentation. For this tutorial we will use the ``Sphinx`` format, since, as the name suggests, it is the standard format used with Sphinx. Other formats include ``Google`` (see `here <https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html>`_) and ``NumPy`` (see `here <http://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html#example-numpy>`_), but they require the use of Sphinx's ``napoleon`` extension, which is beyond the scope of this tutorial.

The Sphinx docstring format
***************************

In general, a typical ``Sphinx`` docstring has the following format:

.. code-block:: python

    """[Summary]

    :param [ParamName]: [ParamDescription], defaults to [DefaultParamVal]
    :type [ParamName]: [ParamType](, optional)
    ...
    :raises [ErrorType]: [ErrorDescription]
    ...
    :return: [ReturnDescription]
    :rtype: [ReturnType]
    """

A pair of ``:param:`` and ``:type:`` directive options must be used for each parameter we wish to document. The ``:raises:`` option is used to describe any errors that are raised by the code, while the ``:return:`` and ``:rtype:`` options are used to describe any values returned by our code. A more thorough explanation of the ``Sphinx`` docstring format can be found `here <https://thomas-cokelaer.info/tutorials/sphinx/docstring_python.html>`_. 

Note that the ``...`` notation has been used above to indicate repetition and should not be used when generating actual docstrings, as can be seen by the example presented below.


An example class with docstrings
******************************** 

Let's have a look at a typical class documentation. In this example we show the docstrings written for the ``SimpleBleDevice`` class, which is defined within our ``simpleble`` module:

.. code-block:: python

    class SimpleBleDevice(object):
        """This is a conceptual class representation of a simple BLE device
        (GATT Server). It is essentially an extended combination of the
        :class:`bluepy.btle.Peripheral` and :class:`bluepy.btle.ScanEntry` classes

        :param client: A handle to the :class:`simpleble.SimpleBleClient` client
            object that detected the device
        :type client: class:`simpleble.SimpleBleClient`
        :param addr: Device MAC address, defaults to None
        :type addr: str, optional
        :param addrType: Device address type - one of ADDR_TYPE_PUBLIC or
            ADDR_TYPE_RANDOM, defaults to ADDR_TYPE_PUBLIC
        :type addrType: str, optional
        :param iface: Bluetooth interface number (0 = /dev/hci0) used for the
            connection, defaults to 0
        :type iface: int, optional
        :param data: A list of tuples (adtype, description, value) containing the
            AD type code, human-readable description and value for all available
            advertising data items, defaults to None
        :type data: list, optional
        :param rssi: Received Signal Strength Indication for the last received
            broadcast from the device. This is an integer value measured in dB,
            where 0 dB is the maximum (theoretical) signal strength, and more
            negative numbers indicate a weaker signal, defaults to 0
        :type rssi: int, optional
        :param connectable: `True` if the device supports connections, and `False`
            otherwise (typically used for advertising ‘beacons’).,
            defaults to `False`
        :type connectable: bool, optional
        :param updateCount: Integer count of the number of advertising packets
            received from the device so far, defaults to 0
        :type updateCount: int, optional
        """

        def __init__(self, client, addr=None, addrType=None, iface=0,
                     data=None, rssi=0, connectable=False, updateCount=0):
            """Constructor method
            """
            super().__init__(deviceAddr=None, addrType=addrType, iface=iface)
            self.addr = addr
            self.addrType = addrType
            self.iface = iface
            self.rssi = rssi
            self.connectable = connectable
            self.updateCount = updateCount
            self.data = data
            self._connected = False
            self._services = []
            self._characteristics = []
            self._client = client

        def getServices(self, uuids=None):
            """Returns a list of :class:`bluepy.blte.Service` objects representing
            the services offered by the device. This will perform Bluetooth service
            discovery if this has not already been done; otherwise it will return a
            cached list of services immediately..

            :param uuids: A list of string service UUIDs to be discovered,
                defaults to None
            :type uuids: list, optional
            :return: A list of the discovered :class:`bluepy.blte.Service` objects,
                which match the provided ``uuids``
            :rtype: list On Python 3.x, this returns a dictionary view object,
                not a list
            """
            self._services = []
            if(uuids is not None):
                for uuid in uuids:
                    try:
                        service = self.getServiceByUUID(uuid)
                        self.services.append(service)
                    except BTLEException:
                        pass
            else:
                self._services = super().getServices()
            return self._services

        def setNotificationCallback(self, callback):
            """Set the callback function to be executed when the device sends a
            notification to the client.

            :param callback: A function handle of the form
                ``callback(client, characteristic, data)``, where ``client`` is a
                handle to the :class:`simpleble.SimpleBleClient` that invoked the
                callback, ``characteristic`` is the notified
                :class:`bluepy.blte.Characteristic` object and data is a
                `bytearray` containing the updated value. Defaults to None
            :type callback: function, optional
            """
            self.withDelegate(
                SimpleBleNotificationDelegate(
                    callback,
                    client=self._client
                )
            )

        def getCharacteristics(self, startHnd=1, endHnd=0xFFFF, uuids=None):
            """Returns a list containing :class:`bluepy.btle.Characteristic`
            objects for the peripheral. If no arguments are given, will return all
            characteristics. If startHnd and/or endHnd are given, the list is
            restricted to characteristics whose handles are within the given range.

            :param startHnd: Start index, defaults to 1
            :type startHnd: int, optional
            :param endHnd: End index, defaults to 0xFFFF
            :type endHnd: int, optional
            :param uuids: a list of UUID strings, defaults to None
            :type uuids: list, optional
            :return: List of returned :class:`bluepy.btle.Characteristic` objects
            :rtype: list
            """
            self._characteristics = []
            if(uuids is not None):
                for uuid in uuids:
                    try:
                        characteristic = super().getCharacteristics(
                            startHnd, endHnd, uuid)[0]
                        self._characteristics.append(characteristic)
                    except BTLEException:
                        pass
            else:
                self._characteristics = super().getCharacteristics(startHnd,
                                                                   endHnd)
            return self._characteristics

        def connect(self):
            """Attempts to initiate a connection with the device.

            :return: `True` if connection was successful, `False` otherwise
            :rtype: bool
            """
            try:
                super().connect(self.addr,
                                addrType=self.addrType,
                                iface=self.iface)
            except BTLEException as ex:
                self._connected = False
                return (False, ex)
            self._connected = True
            return True

        def disconnect(self):
            """Drops existing connection to device
            """
            super().disconnect()
            self._connected = False

        def isConnected(self):
            """Checks to see if device is connected

            :return: `True` if connected, `False` otherwise
            :rtype: bool
            """
            return self._connected

        def printInfo(self):
            """Print info about device
            """
            print("Device %s (%s), RSSI=%d dB" %
                  (self.addr, self.addrType, self.rssi))
            for (adtype, desc, value) in self.data:
                print("  %s = %s" % (desc, value))


Once processed by ``autodoc`` the generated documentation for the above class looks like `this <http://simpleble.readthedocs.io/en/latest/simpleble.html#the-simplebledevice-class>`_.

Docstrings in VS code
*********************

If you are using VS code, the `Python Docstring <https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring>`_ extension can be used to auto-generate a docstring snippet once a function/class has been written. If you want the extension to generate docstrings in ``Sphinx`` format, you must set the ``"autoDocstring.docstringFormat": "sphinx"`` setting, under File > Preferences > Settings. 

Note that it is best to write the docstrings once you have fully defined the function/class, as then the extension will generate the full dosctring. If you make any changes to the code once a docstring is generated, you will have to manually go and update the affected docstrings.


