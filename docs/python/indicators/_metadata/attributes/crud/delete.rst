Delete Indicator Attributes
"""""""""""""""""""""""""""

The code snippet below demonstrates how to delete an Indicator's attribute. This example assumes a host indicator ``example.com`` exists in the target owner.

.. code-block:: python
    :emphasize-lines: 34,36-37

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Indicators object
    indicators = tc.indicators()

    # set a filter to retrieve a specific host indicator: example.com
    filter1 = indicators.add_filter()
    filter1.add_indicator('example.com')

    try:
        # retrieve the Indicators
        indicators.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the Indicators
    for indicator in indicators:
        print(indicator.indicator)

        # load the indicator's attributes
        indicator.load_attributes()

        # iterate through the indicator's attributes
        for attribute in indicator.attributes:
            print(attribute.id)

            # if the current attribute is a description attribute, delete it
            if attribute.type == 'Description':
                indicator.delete_attribute(attribute.id)

        # commit the changes
        indicator.commit()

.. note:: In the prior example, no API calls are made until the ``commit()`` method is invoked.
