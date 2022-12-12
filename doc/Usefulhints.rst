Useful Hints
============

Running the model on a Microsoft Virtual Machine (VM)
-----------------------------------------------------

Since this program is very resource-intensive it can be useful to run this on a Microsoft VM to free up resources on the local machine. Since some coorporate VM's will automatically time out when no activity is detected, which is defined as no movement by the mouse or keyboard, it will time out and close all programs that are currently running. This is, of course, problematic when the program is required to run overnight or a couple of days. This can be prevented by doing the following

#. Open ``PowerShell``
#. Copy the following line in the command propmpt:

.. code-block:: console

    $wsh = New-Object -ComObject WScript.Shell; while (1) {$wsh.SendKeys('+{F15}'); Start-Sleep -seconds 59}

#. Keep the ``PowerShell`` window open until the program has finished and you want to log off

For more information check out the original source `jamesfreeman959/keepawake.ps1 <https://gist.github.com/jamesfreeman959/231b068c3d1ed6557675f21c0e346a9c>_`