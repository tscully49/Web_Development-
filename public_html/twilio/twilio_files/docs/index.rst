.. Services_Twilio documentation master file, created by
   sphinx-quickstart on Tue Mar  8 04:02:01 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

=================
**twilio-php**
=================

Status
=======

This documentation is for version 4.5.0 of `twilio-php
<https://www.github.com/twilio/twilio-php>`_.

Quickstart
============

Send an SMS
>>>>>>>>>>>

.. code-block:: php

    // Download the library and copy into the folder containing this file.
    require('/path/to/twilio-php/Services/Twilio.php');

    $account_sid = "ACXXXXXX"; // Your Twilio account sid
    $auth_token = "YYYYYY"; // Your Twilio auth token

    $client = new Services_Twilio($account_sid, $auth_token);
    $message = $client->account->messages->sendMessage(
      '+14085551234', // From a Twilio number in your account
      '+12125551234', // Text any number
      "Hello monkey!"
    );

    print $message->sid;

Make a Call
>>>>>>>>>>>>>>

.. code-block:: php

    // Download the library and copy into the folder containing this file.
    require('/path/to/twilio-php/Services/Twilio.php');

    $account_sid = "ACXXXXXX"; // Your Twilio account sid
    $auth_token = "YYYYYY"; // Your Twilio auth token

    $client = new Services_Twilio($account_sid, $auth_token);
    $call = $client->account->calls->create(
      '+14085551234', // From a Twilio number in your account
      '+12125551234', // Call any number

      // Read TwiML at this URL when a call connects (hold music)
      'http://twimlets.com/holdmusic?Bucket=com.twilio.music.ambient'
    );

Generating TwiML
>>>>>>>>>>>>>>>>

To control phone calls, your application needs to output `TwiML
<http://www.twilio.com/docs/api/twiml/>`_. Use :class:`Services_Twilio_Twiml`
to easily create such responses.

.. code-block:: php

    $response = new Services_Twilio_Twiml();
    $response->say('Hello');
    $response->play('https://api.twilio.com/cowbell.mp3', array("loop" => 5));
    print $response;

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <Response>
        <Say>Hello</Say>
        <Play loop="5">https://api.twilio.com/cowbell.mp3</Play>
    </Response>

View more examples of TwiML generation here: :ref:`usage-twiml`

Creating a Task
===================

You can easily create a task. Tasks can represent whatever type of work is important for your team. Twilio applications can create tasks from phone calls or SMS messages. 

.. code-block:: php

    require 'Services/Twilio.php';

    $accountSid = 'YOUR_ACCOUNT_SID';
    $authToken = 'YOUR_AUTH_TOKEN';
    $workspaceSid = 'YOUR_WORKSPACE_SID';

    // instantiate a Twilio TaskRouter Client 
    $taskrouterClient = new TaskRouter_Services_Twilio($accountSid, $authToken, $workspaceSid);
    
    // set task parameters
    $workflowSid = 'YOUR_WORKFLOW_SID'; 
    $taskAttributes = '{"selected_language": "fr"}';
    $priority = 10; 
    $timeout = 100;  

    // create task
    $task = $taskrouterClient->workspace->tasks->create(
        $taskAttributes, 
        $workflowSid, 
        array(
            'Priority' => $priority, 
            'Timeout' => $timeout
        )
    ); 
    
    // confirm task created
    echo "Created Task: ".$task->sid; 

Find more examples and details of TaskRouter here: :ref:`usage-taskrouter`

Installation
============

There are two ways to install **twilio-php**: via Composer, or by
downloading the source.

Via Composer
>>>>>>>>>>>>>

.. code-block:: bash

	composer require 'twilio/sdk'

From Source
>>>>>>>>>>>>>

If you aren't using Composer, download the `source (.zip)
<https://github.com/twilio/twilio-php/zipball/master>`_, which includes all the
dependencies.

User Guide
==================

REST API
>>>>>>>>>>

.. toctree::
    :maxdepth: 2
    :glob:

    usage/rest
    usage/rest/*

TwiML and other utilities
>>>>>>>>>>>>>>>>>>>>>>>>>>

.. toctree::
    :maxdepth: 1

    usage/twiml
    usage/validation
    usage/token-generation
    faq/

TaskRouter Reference
>>>>>>>>>>>>>>>>>>>>>

.. toctree::
    :maxdepth: 2
    :glob:

    usage/taskrouter
    usage/taskrouter/*

API Documentation
==================

.. toctree::
    :maxdepth: 3
    :glob:

    api/*


Support and Development
===========================

All development occurs on `Github <https://github.com/twilio/twilio-php>`_. To
check out the source, run

.. code-block:: bash

    git clone git@github.com:twilio/twilio-php.git

Report bugs using the Github `issue tracker <https://github.com/twilio/twilio-php/issues>`_.

If you've got questions that aren't answered by this documentation, ask the
Twilio support team at help@twilio.com.

Running the Tests
>>>>>>>>>>>>>>>>>>>>>>>>>

The unit tests depend on `Mockery <https://github.com/padraic/mockery>`_ and
`PHPUnit <https://github.com/sebastianbergmann/phpunit>`_. First, 'discover' all
the necessary `PEAR` channels:

.. code-block:: bash

    make test-install

After installation, run the tests with :data:`make`.

.. code-block:: bash

    make test


Making the Documentation
>>>>>>>>>>>>>>>>>>>>>>>>>>

Our documentation is written using `Sphinx <http://sphinx.pocoo.org/>`_. You'll
need to install Sphinx and the Sphinx PHP domain before you can build the docs.

.. code-block:: bash

    make docs-install

Once you have those installed, making the docs is easy.

.. code-block:: bash

    make docs

