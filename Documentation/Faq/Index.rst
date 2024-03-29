.. include:: ../Includes.txt
.. include:: Images.txt

.. _faq:

FAQ
===

.. _caniuseoldmails:

Can I use old mails in powermail 2.x?
-------------------------------------

No. It's not possible to use old powermail mails with the new module. Old mails are stored in table tx_powermail_mails. This table is not accepted any more in version 2.0 or higher.

.. _caniuseoldforms:

Can I use old forms in powermail 2.x?
-------------------------------------

Yes. You can convert old forms (1.x) to version 2.x in powermail 2.1. Please use the related powermail backend module.


.. _canisueanothercaptcha:

Can I use another Captcha Extension?
------------------------------------

No. At the moment we support only a calculating captcha in the powermail core. Maybe other extensions in a later version.


.. _canisavetoothertables:

Can I save values to tt_address, fe_users, tt_news, etc...?
-----------------------------------------------------------

Yes. It's very easy to save values to a third-party-table – see manual part

For Administrators / Good to know / Saving Values to Third Party Table :ref:`savingvaluestothirdpartytables`


.. _caniwritemyownvalidator:

Can I write my own javascript/php validator?
--------------------------------------------

Yes. Write your own validator – see manual part
For Developers / Write own JavaScript Validator and For Developers / Write own PHP Validation :ref:`newvalidators`



.. _caniattachfilestoanymail:

Can I attach files to any mail?
-------------------------------

Yes. You can simply add some files to any mail via TypoScript cObject – see TypoScript Main Template for details.

Short example:

.. code-block:: text

	plugin.tx_powermail.settings.setup.sender {
		addAttachment = TEXT
		addAttachment.value = fileadmin/file.pdf
	}


.. _howadminconfirmdoubleoptinmail:

How can the admin confirm a mail from Double-Opt-In?
----------------------------------------------------

Yes. Per default the confirmation Email (if Double-Opt-In is enabled) will be
sent to the sender.

You can overwrite it via TypoScript. See TypoScript Main Template for details.

Short example:

.. code-block:: text

	plugin.tx_powermail.settings.setup.optin {
		overwrite {
			email = TEXT
			email.value = admin@domain.org
		}
	}



.. _howtopreventspam:

How to prevent Spam or to change the Spam-Prevention-Settings?
--------------------------------------------------------------

Yes. Powermail in version 2 comes with a lot of spam-prevention-methods along.
You can use the integrated spamshield (configuration via constants and typoscript)
or captcha. See TypoScript Main Template for details.



.. _howtosetadvancedemailsettings:

How can I set some advanced mail settings (like priority or returnPath, etc...)?
--------------------------------------------------------------------------------

You can change following settings for the mail to the receiver and to
the sender completely via TypoScript. See TypoScript Main Template for
details.

- email
- name
- senderName
- senderEmail
- subject
- cc Receivers
- bcc Receivers
- returnPath
- reply to email
- reply to name
- priority


How to change the style selector with my own values (In Forms, Pages or Fields)?
--------------------------------------------------------------------------------

.. code-block:: html

	<select>
		<option value=”layout1”>Layout1</option>
		<option value=”layout2”>Layout2</option>
		<option value=”layout3”>Layout3</option>
	</select>



Changing values via page tsconfig
'''''''''''''''''''''''''''''''''

.. code-block:: text

	TCEFORM {
		tx_powermail_domain_model_forms {
			css {
				removeItems = layout1, layout2, layout3
				addItems {
					blue = Blue Form
					green = Green Form
				}
			}
		}
		tx_powermail_domain_model_pages < tx_powermail_domain_model_forms
		tx_powermail_domain_model_fields < tx_powermail_domain_model_forms
	}




This configuration produces this output:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: html

	<select>
		<option value=”blue”>Blue Form</option>
		<option value=”green”>Green Form</option>
	</select>

And adds the class “blue” or “green” to all forms, pages and fields in
the Frontend.

|img-93|


.. _howtohidefieldsforeditors:

How to hide fields for editors?
-------------------------------

Hiding field from tables form, page or field
''''''''''''''''''''''''''''''''''''''''''''

For editors (not administrators) fields can be disabled in the right management of TYPO3 (see TYPO3 documentation how to show or hide fields for editors).

Another way is to hide fields for editors (and administrators) via Page TSConfig:

.. code-block:: text

	TCEFORM {
		tx_powermail_domain_model_forms {
			css.disabled = 1
		}
		tx_powermail_domain_model_pages {
			css.disabled = 1
		}
		tx_powermail_domain_model_fields {
			css.disabled = 1
			feuser_value.disabled = 1
			placeholder.disabled = 1
		}
	}

Hiding fields from FlexForm
'''''''''''''''''''''''''''

If you add a powermail plugin, you will see some options in FlexForm. If you want to hide some of theese fields (for editors and administrators), you can also do it via Page TSConfig:

.. code-block:: text

	TCEFORM {
		tt_content {
			pi_flexform {
				powermail_pi1 {
					main {
						settings\.flexform\.main\.moresteps.disabled = 1
						settings\.flexform\.main\.optin.disabled = 1
						settings\.flexform\.main\.confirmation.disabled = 1
						settings\.flexform\.main\.pid.disabled = 1
					}

					receiver {
						settings\.flexform\.receiver\.fe_group.disabled = 1
					}

					thx {
						settings\.flexform\.thx\.redirect.disabled = 1
					}
				}
			}
		}
	}



.. _howtoprefillfields:

How to prefill a field in the powermail form?
---------------------------------------------

Prefilling of fields will be done by the prefillFieldsViewHelper. It
listen to the following methods and parameters:

# GET/POST param like &tx\_powermail\_pi1[marker]=value
# GET/POST param like &tx\_powermail\_pi1[field][123]=value
# GET/POST param like &tx\_powermail\_pi1[uid123]=value
# If field should be filled with values from FE\_User (Flexform Settings)
# If field should be prefilled from static Flexform Setting
# Fill with TypoScript cObject like

.. code-block:: text

	plugin.tx_powermail.settings.setup.prefill {
		# Fill field with marker {email}
		email = TEXT
		email.value = mail@domain.org
	}

7. Fill with TypoScript like

.. code-block:: text

	plugin.tx_powermail.settings.setup.prefill {
		# Fill field with marker {email}
		email = mail@domain.org
	}


.. _howdonotincludejquery:

How can I include jQuery?
-------------------------

In powermail 2.0 and smaller, jQuery was included automaticly. Since 2.1, you have to enable this feature via Constants:

.. code-block:: text

	plugin.tx_powermail.settings {
		javascript {
			addJQueryFromGoogle = 1
			powermailJQuery = //ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js
		}
	}


.. _howcaniuset3jquery:

How can I use t3jquery?
-----------------------

No, this is natively not possible. You can manually disable the including of the
libraries from google (see line above) and force to use jQuery from t3jquery (see
manual of that extension)

.. _javascriptvalidationdoesnotwork:

JavaScript validation does not work – what's wrong?
---------------------------------------------------

At the moment we do not use t3jquery. Powermail loads jQuery (if you activated it with TypoScript) from googleapis.com.
You can change that behaviour with constants or typoscript.

It's importand to have the correct ordering of the JavaScript files.
First you need the libraries (jQuery, Datepicker, Parsley) and after that your JavaScript.

Check the correct including of your JavaScript in the HTML source –
example Footer could be:

.. code-block:: html

	<script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js" type="text/javascript"></script>
	<script src="typo3conf/ext/powermail/Resources/Public/JavaScripts/jquery.datetimepicker.js?1400758352" type="text/javascript"></script>
	<script src="typo3conf/ext/powermail/Resources/Public/JavaScripts/parsley.min.js?1400758352" type="text/javascript"></script>
	<script src="typo3conf/ext/powermail/Resources/Public/JavaScripts/tabs.js?1400758352" type="text/javascript"></script>
	<script src="typo3conf/ext/powermail/Resources/Public/JavaScripts/form.js?1400758352" type="text/javascript"></script>
	<script src="typo3conf/ext/powermail/Resources/Public/JavaScripts/Marketing.js?1400758352" type="text/javascript"></script>
	<script src="typo3conf/ext/powermail/Resources/Public/JavaScripts/powermail_frontend.js?1400758352" type="text/javascript"></script>


.. _automaticexportviascheduler:

I want to have automatic export files with the scheduler module – what's wrong?
-------------------------------------------------------------------------------

Nothing. It is just not yet possible. In powermail 1.4 up to 1.7 it was possible
to get automatic export files from a cli script or from the scheduler. This feature is
not yet integrated in version 2.x.

.. _marketinginformationnotworking:

Marketing Information are not working – what's wrong?
-----------------------------------------------------

Did you include the marketing static template on the root page of your
domain?

.. _addnewfieldtype:

I want to add a new Field Type to powermail – how can I do this
---------------------------------------------------------------

Yes, you can add a new Fieldtype (in record
tx\_powermail\_domain\_model\_fields) with some Page TSConfig.

See following example to add a new fieldtype with Partial Newfield.html (see Documentation/ForDevelopers/NewField for an advanced example)

.. code-block:: text

	tx_powermail.flexForm.type.addFieldOptions.newfield = New Field Name

.. _ihaveaproblem:

I have a problem, what can I do?
--------------------------------

- Did you read the manual?
- Turning on the Debug Output in Powermail (via TypoScript) can help you to find a solution (please use extension devlog to look into the debug arrays)
- Try to get free help from a TYPO3 Forum like (typo3.org, typo3.net or typo3forum.net)
- Do you need payed support? Please write to http://www.in2code.de
- Did you find a bug? Report it to http://forge.typo3.org/projects/extension-powermail/issues
- Did you miss a feature? Feel free to write it down also to forge http://forge.typo3.org/projects/extension-powermail/issues


