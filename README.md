neogeo987
=========

This plugin is not my code. It is basically a fork from a plugin by phpsa: https://github.com/phpsa/cbsp
At the time of creating this plugin, Phonegap Build would not accept the plugin by phpsa, because it contained a compiled binary file.
This fork of that plugin is an attempt to get the plugin "Phonegap Build Ready".

cbsp
====

Cordova Background Services Plugin based on https://github.com/Red-Folder/Cordova-Plugin-BackgroundService,
A plugin (and framework code) that allows the development and operation of an Android Background Service.

The example MyService Background Service will write a Hello message to the LogCat every minute. The MyService is designed as sample code.

Once installed you will need to edit the com.red_folder.phonegap.plugin.backgroundservice.MyService.java file to make changes.

If you find the Background Service Plugin useful then spread the love.
https://github.com/Red-Folder/Cordova-Plugin-BackgroundService#spread-the-love

See

Change in the example index.html the following line

    var myService = cordova.require('cordova/plugin/myService');

to

    var myService = cordova.require('com.red_folder.phonegap.plugin.backgroundservice.BackgroundService');



Example::

    <!DOCTYPE HTML>
    <!--
    /*
     * Copyright 2013 Red Folder Consultancy Ltd
     *   
     * Licensed under the Apache License, Version 2.0 (the "License");   
     * you may not use this file except in compliance with the License.   
     * You may obtain a copy of the License at       
     * 
     * 	http://www.apache.org/licenses/LICENSE-2.0   
     *
     * Unless required by applicable law or agreed to in writing, software   
     * distributed under the License is distributed on an "AS IS" BASIS,   
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.   
     * See the License for the specific language governing permissions and   
     * limitations under the License.
     */
    -->
    <html>
        <head>
            <title>MyService V3.1.0</title>

            <script type="text/javascript" charset="utf-8" src="cordova.js"></script>
            <script type="text/javascript" charset="utf-8" src="backgroundService-3.1.0.js"></script>
            <script type="text/javascript">
                /*
                 * Copyright 2013 Red Folder Consultancy Ltd
                 *   
                 * Licensed under the Apache License, Version 2.0 (the "License");   
                 * you may not use this file except in compliance with the License.   
                 * You may obtain a copy of the License at       
                 * 
                 * 	http://www.apache.org/licenses/LICENSE-2.0   
                 *
                 * Unless required by applicable law or agreed to in writing, software   
                 * distributed under the License is distributed on an "AS IS" BASIS,   
                 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.   
                 * See the License for the specific language governing permissions and   
                 * limitations under the License.
                 */




            </script>

            <script type="text/javascript" >

                var myService = cordova.require('com.red_folder.phonegap.plugin.backgroundservice.BackgroundService');



                document.addEventListener('deviceready', function() {
                    getStatus();
                }, true);

                function handleSuccess(data) {
                    updateView(data);
                }

                function handleError(data) {
                    alert("Error: " + data.ErrorMessage);
                    alert(JSON.stringify(data));
                    updateView(data);
                }

                /*
                 * Button Handlers
                 */
                function getStatus() {
                    myService.getStatus(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }
                ;

                function startService() {
                    myService.startService(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }

                function stopService() {
                    myService.stopService(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }

                function enableTimer() {
                    myService.enableTimer(60000,
                            function(r) {
                                handleSuccess(r)
                            },
                            function(e) {
                                handleError(e)
                            });
                }

                function disableTimer() {
                    myService.disableTimer(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }
                ;

                function registerForBootStart() {
                    myService.registerForBootStart(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }

                function deregisterForBootStart() {
                    myService.deregisterForBootStart(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }

                function registerForUpdates() {
                    myService.registerForUpdates(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }

                function deregisterForUpdates() {
                    myService.deregisterForUpdates(function(r) {
                        handleSuccess(r)
                    },
                            function(e) {
                                handleError(e)
                            });
                }

                function setConfig() {
                    var helloToTxt = document.getElementById("helloToTxt");
                    var helloToString = helloToTxt.value;
                    var config = {
                        "HelloTo": helloToString
                    };
                    myService.setConfiguration(config,
                            function(r) {
                                handleSuccess(r)
                            },
                            function(e) {
                                handleError(e)
                            });
                }

                /*
                 * View logic
                 */
                function updateView(data) {

                    console.log("UPDATE VIEW CALLED!!!!");

                    var serviceBtn = document.getElementById("toggleService");
                    var timerBtn = document.getElementById("toggleTimer");
                    var bootBtn = document.getElementById("toggleBoot");
                    var listenBtn = document.getElementById("toggleListen");
                    var updateBtn = document.getElementById("updateBtn");
                    var refreshBtn = document.getElementById("refreshBtn");

                    var serviceStatus = document.getElementById("serviceStatus");
                    var timerStatus = document.getElementById("timerStatus");
                    var bootStatus = document.getElementById("bootStatus");
                    var listenStatus = document.getElementById("listenStatus");

                    serviceBtn.disabled = false;
                    if (data.ServiceRunning) {
                        serviceStatus.innerHTML = "Running";
                        serviceBtn.onclick = stopService;
                        timerBtn.disabled = false;
                        if (data.TimerEnabled) {
                            timerStatus.innerHTML = "Enabled";
                            timerBtn.onclick = disableTimer;
                        } else {
                            timerStatus.innerHTML = "Disabled";
                            timerBtn.onclick = enableTimer;
                        }

                        updateBtn.disabled = false;
                        updateBtn.onclick = setConfig;

                        refreshBtn.disabled = false;
                        refreshBtn.onclick = getStatus;

                    } else {
                        serviceStatus.innerHTML = "Not running";
                        serviceBtn.onclick = startService;
                        timerBtn.disabled = true;
                        timerEnabled = false;

                        updateBtn.disabled = true;
                        refreshBtn.disabled = true;
                    }

                    bootBtn.disabled = false;
                    if (data.RegisteredForBootStart) {
                        bootStatus.innerHTML = "Registered";
                        bootBtn.onclick = deregisterForBootStart;
                    } else {
                        bootStatus.innerHTML = "Not registered";
                        bootBtn.onclick = registerForBootStart;
                    }

                    listenBtn.disabled = false;
                    if (data.RegisteredForUpdates) {
                        listenStatus.innerHTML = "Registered";
                        listenBtn.onclick = deregisterForUpdates;
                    } else {
                        listenStatus.innerHTML = "Not registered";
                        listenBtn.onclick = registerForUpdates;
                    }

                    if (data.Configuration != null)
                    {
                        try {
                            var helloToTxt = document.getElementById("helloToTxt");
                            helloToTxt.value = data.Configuration.HelloTo;
                        } catch (err) {
                        }
                    }

                    if (data.LatestResult != null)
                    {
                        try {
                            var resultMessage = document.getElementById("resultMessage");
                            resultMessage.innerHTML = data.LatestResult.Message;
                        } catch (err) {
                        }
                    }
                }

            </script>

        </head>

        <body>
            <h1>MyService V3.1.0</h1>

            <table>
                <tr>
                    <th>Service</th>
                    <td><div id="serviceStatus"></div></td>
                    <td><input disabled id="toggleService" type="button" value="toggle"/></td>
                </tr>
                <tr>
                    <th>Timer</th>
                    <td><div id="timerStatus"></div></td>
                    <td><input disabled id="toggleTimer" type="button" value="toggle"/></td>
                </tr>
                <tr>
                    <th>Boot</th>
                    <td><div id="bootStatus"></div></td>
                    <td><input disabled id="toggleBoot" type="button" value="toggle"/></td>
                </tr>
                <tr>
                    <th>Listen</th>
                    <td><div id="listenStatus"></div></td>
                    <td><input disabled id="toggleListen" type="button" value="toggle"/></td>
                </tr>

                <tr>
                    <th colspan=3 align="center">Configuration</th>
                </tr>
                <tr>
                    <th align="left">Hello To</th>
                    <td colspan=2 align="center"><input id="helloToTxt" type="Text"/></td>
                </tr>
                <tr>
                    <td colspan=3 align="center"><input disabled id="updateBtn" type="button" value="Update Config"/></td>
                </tr>

                <tr>
                    <th colspan=3 align="center">Latest Result</th>
                </tr>

                <tr>
                    <td colspan=3 align="center"><div id="resultMessage"></div></td>
                </tr>

                <tr>
                    <td colspan=3 align="center"><input disabled id="refreshBtn" type="button" value="Refresh"/></td>
                </tr>

            </table>

        </body>
    </html>


Support:
The code is all taken from https://github.com/Red-Folder/Cordova-Plugin-BackgroundService and is theirs,
We will offer support where possible however we have simply wrapped the code to allow for it to work within the
cordova CLI.
