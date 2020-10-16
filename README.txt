SAMPLE

   Simple Predictive Analytics sample to demostrate use of ModelManager interface.

   ModelManager can be used to dynamically add new models, update an existing model
   or remove a model from a predictive analytics plugin instance.


COPYRIGHT NOTICE

   $Copyright (c) 2015-2017 Software AG, Darmstadt, Germany and/or Software AG USA Inc., Reston, VA, USA, and/or its subsidiaries and/or its affiliates and/or their licensors.$
   Use, reproduction, transfer, publication or disclosure is prohibited except as specifically provided for in your License Agreement with Software AG


FILES

   README.txt                                                     : This file
   build.xml                                                      : Ant build file
   config\launch\EnergyData Model Manager Sample.{deploy,launch}  : Software AG Designer launch configuration
   events\EnergyData.evt                                          : Scoring data used during sample execution
   model\EnergyDataModel.pmml                                     : PMML model used in the sample
   model\EnergyDataModel_Updated.pmml                             : Updated PMML model to be used in the sample
   monitors\EnergyData_ModelMgrSample.mon                         : Apama EPL application to initialise the Plugin
                                                                    instance and send requests to score data


PRE-REQUISITES

   It is recommended that you copy this sample folder to an area of your APAMA_WORK directory rather
   than running it directly from the installation directory. For Windows users with UAC enabled this
   step is required to avoid access denied errors when writing to the sample directory.

   Please ensure the Predictive Analytics Engine license file ('zementis.license') is copied to
   APAMA_WORK/license folder.


RUNNING THE SAMPLE

   The sample performs the following tasks:

      1. Start Correlator, injects the plugin bundle, initialises the plugin
         by injecting EnergyData_ModelMgrSample.mon and input is sent from EnergyData.evt.

      2. Apama EPL application EnergyData_ModelMgrSample.mon does the following to configure the plugin:

         a. Uses a ServiceHandlerFactory to create a new Predictive Analytics Plugin Instance with
            the configured parameters.

         b. Once the predictive analytics plugin instance is successfully created a PMML model is
            loaded using the ModelManager API provided by ServiceHandler.getModelManager().

         d. The first batch of input requests are scored using the newly added model.

         e. After a wait of 5 seconds, ModelManager is requested to update an
            existing model. i.e. ModelManager.updateModel(<MODEL_NAME>, <path_to_file>)

         f. The second batch of input requests (BATCH 5000 in EnergyData.evt) is then scored using
            the updated model.

         g. Finally ModelManager.removeModel(<MODEL_NAME>) is called to remove an existing model


   To run the sample as an Apama project in Software AG Designer:

   1. Open Software AG Designer.

   2. Select File --> Import.

   3. In the Import dialog, select and expand the General node, then select
      Existing Projects into Workspace.

   4. Click the Next button, and in the Import Project step click the Browse
      button.

   5. Navigate to the folder that contains this README.txt and select that
      folder.

   6. In the Options panel of the Import Projects dialog, check the Copy
      projects into workspace check box.

   7. The project should now be imported into Software AG Designer.

   8. Now run the Apama project by right-clicking it and selecting
      "Run As --> Apama Application".

   9. The output of the sample can be seen in the Console view.

   Running samples using ant configuration:

   1. Open a new Apama Command Prompt on Windows, or on Unix, source the
      apama_env script.

   2. Navigate to the folder that contains this README.txt

   3. To start the correlator and inject predictive analytics plugin and run the
      application, run:

      > ant start

   4. To unload the application and stop the correlator, run:

      > ant stop

   If using the command line:

      To ensure that the environment is configured correctly for Apama, all the
      commands below should be executed from an Apama Command Prompt, or from a
      shell or command prompt where the bin\apama_env script has been run (or on
      Unix, sourced).

      1. Start the Apama Correlator with java support enabled in the Apama
         Command Prompt by running:

         > correlator --java

         The Apama Command Prompt can be started using the Windows Start Menu
         shortcut.

      2. In a separate command prompt/terminal, inject the required monitors:

         > engine_inject --java "%APAMA_HOME%\adapters\lib\Predictive-Analytics-Plugin.jar"
         > engine_inject --cdp "%APAMA_HOME%\adapters\monitors\predictive_analytics_plugin_monitors.cdp"

         on Windows, or on Unix:

         > engine_inject --java "$APAMA_HOME/adapters/lib/Predictive-Analytics-Plugin.jar"
         > engine_inject --cdp "$APAMA_HOME/adapters/monitors/predictive_analytics_plugin_monitors.cdp"

      3. Make sure the current directory is the directory where this sample is
         located, and then inject the following MonitorScript file to run the
         sample:

         > engine_inject "monitors/EnergyData_ModelMgrSample.mon"

      4. Make sure the current directory is the directory where this sample is
         located, and then send the following as input to the sample:

         > engine_send "events/EnergyData.evt"


SAMPLE OUTPUT

   The Correlator log should show two groups of messages similar to the following:

     com.apama.pa.pmml.sample.PredictiveAnalytics_ModelManager_Sample [1] {"Predicted_Usage":"16.18362364781374"}
     com.apama.pa.pmml.sample.PredictiveAnalytics_ModelManager_Sample [1] {"Predicted_Usage":"15.397684338406936"}
     com.apama.pa.pmml.sample.PredictiveAnalytics_ModelManager_Sample [1] {"Predicted_Usage":"19.12970126490951"}
     com.apama.pa.pmml.sample.PredictiveAnalytics_ModelManager_Sample [1] {"Predicted_Usage":"15.796460465819097"}
     com.apama.pa.pmml.sample.PredictiveAnalytics_ModelManager_Sample [1] {"Predicted_Usage":"21.046370444450062"}

     The second set of results correspond to the same set of input requests being scored using the update model

