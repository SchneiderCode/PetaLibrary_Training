<!--

@onload
window.sendData = async function sendData({url='https://script.google.com/macros/s/AKfycbzoho4vFdv5J2e06WGnIOhRW5O0VPAMeMRK9t55oyiGXMJo8Z0OlSC-hIS2Ju30le3xUA/exec', username, email, course, question, value}) {
   //This script will transmit the user's credentials to the associated Google App Script & Google Sheet
   // TODO - Make sure to add new tabs to the training sheet (See Confluence Page for Link to Google Sheet)
     // <TRAINING_NAME>_SCORES - Tracks when a user correctly answers a question / knowledge check
     // <TRAINING_NAME>_VISITS - Tracks when a user visits a section of the training
   //You can instead create a new Google Sheet for tracking course interaction using the template linked in Confluence, but make sure to update url above to use the new Google Sheet's App Script
   
  //Only transmit data if a valid email has been given
  if(email.endsWith(".edu") || email.endsWith(".gov") || email.startsWith("anon_")){
    // We inject the captured selection into your JSON
    const payload = `{
        "username" : "${username}",
        "email" : "${email}", 
        "course" : "${course}",
        "question" : "${question}",
        "value" : "${value}"
    }`;

    try {
        const response = await fetch(url, {
            method: 'POST',
            mode: 'no-cors',
            headers: {
                'Content-Type': 'application/json',
            },
            body: payload,
        });
        
        // Note: With 'no-cors', we cannot read response.text(), but the request sends.
        console.log("Request sent successfully to Google Sheets");
        send.lia("true")
        return "Submission Successful"; 
    } catch (error) {
        console.error("Error:", error);
        alert(error)
        send.lia("Update Failed", [], false)
        return "Error sending data";
    }
  }
  else{
    console.log("Invalid Email")
  }
}

window.user_name="anon_" + Math.floor(Math.random() * 1000000);
window.user_email=window.user_name + "email.edu"

@end

persistent: true

icon: img/rc_logo.png

version: 0.0.1

-->

# Research Computing PetaLibrary Training

Unlock the potential of your data with this introductory training on Research Computing's PetaLibrary service, designed to help you efficiently store, share, and archive research data. You will learn:

 * about the different storage tiers
 * how to manage data access and file permissions
 * and how to integrate PetaLibrary into your active research workflows! 

<div style="width:45%; margin: 15px 2.5%; float:left;">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

**Please enter your Research Computing account information: **

Do you have an existing RC account? 

<script input="radio" value="Select Yes or No" options="YES|NO">
  //If yes, reveal the username option
  if("@input" === "YES"){
    document.getElementById("username").hidden = false;
    "YES"
  }
  else if ("@input" === "NO"){
    document.getElementById("username").hidden = true;
    "NO"
  }
  else{
    "Select Yes or No"
  }
 
</script>

<div id="username" hidden>

Research Computing username: 

<script input="text" placeholder="buff1234" >
  let user_name_temp = "@input"
  if(user_name_temp){
    user_name = user_name_temp
    user_name_temp
  }
  else{
    "Enter username"
  }

</script>
</div>

Institutional email address: 

<script input="email" placeholder="e.g. Ralphie@colorado.edu" >
  let user_email_temp = "@input"

  if(user_email_temp){
    if(user_email_temp.endsWith(".edu") || user_email_temp.endsWith(".gov")){
      user_email = user_email_temp
    }
    else{
      document.getElementById("email_warning").innerHTML="WARNING - Please enter an institutional email (.edu) or government email (.gov)"
    }
    user_email_temp
  }
  else{
    "Enter institutional email"
  }

</script>

<div id="email_warning" style="color:red">

</div>

<br>



<script input="submit" default="Submit" style="display:block; text-align:center;"  >
  if(user_email.endsWith(".edu") || user_email.endsWith(".gov")){
    let currentDate = new Date();
    sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"START", value: currentDate.toLocaleString()})
    "Information Saved"
  }
  else{
    "Please enter a valid institutional email"
  }
</script>


</div>

<div style="clear:both"></div>

> [!NOTE] 
> Multiple questions are embedded in this training. It is ok if you don't know the answer to every question! Many of the questions are designed to test for common misconceptions and help you avoid common pitfalls for new users. 

> [!IMPORTANT] 
> We aim to make our online resources accessible to everyone. If you encounter any barriers in the materials contained in this tutorial, please report them through our [support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).

---

## Overview

<script hidden>
 let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_VISITS", question:"OVERVIEW", value: currentDate.toLocaleString()})
  "LIA: wait"
</script>

The PetaLibrary is a University of Colorado Boulder Research Computing service that supports the storage, archival, and sharing of research data. It is available to any researcher affiliated with the University of Colorado System (Boulder, Anschutz, Denver, Colorado Springs) at an internal cost rate. It is available at an external cost rate to researchers from other RMACC institutions. 

**Key Points for PetaLibrary Storage:**  

 * Data storage is purchased in TeraByte (TB) units at an annual rate. For reference, 1 TeraByte (TB) = 1,000 GigaBytes (GB).
 * All data stored in PetaLibrary must adhere to the [PetaLibrary Terms of Service](https://www.colorado.edu/rc/resources/petalibrary/tos). 
 * A PetaLibrary allocation on its own is a single copy of your data that is not backed up. Details and options for backing up your research data are described in the [PetaLibrary Allocation Tier Documentation](https://curc.readthedocs.io/en/latest/petalibrary/allocation_types.html).


<div style="display: flex; align-items:center; padding:1em; border-top: dashed 1px; border-bottom: dashed 1px; " >

  <img alt="Read the Docs Logo" src="img/RTD_Logo_Dark.svg" style="width:150px; margin-right:15px; background-color:white; border-radius:5px; padding:5px;"> 

  <p style="margin-bottom:0;" > This tutorial provides a quick overview of CURC's PetaLibrary service. For a more detailed information, make sure to review [CURC's PetaLibrary Documentation](https://curc.readthedocs.io/en/latest/petalibrary/index.html) </p>

</div>

---

## Creating an Allocation

Step 1: Choose the allocation tier that is right for you.

Step 2: Request a new PetaLibrary allocation (links to Docusign form)

Step 3: Learn to use PetaLibrary via this documentation.

TODO - Convert into an infographic / diagram. 

---

## PetaLibrary Tiers

**Active Tier** 

PetaLibrary active allocations are the most performant PetaLibrary tier (please note that performant is a relative term – our parallel scratch filesystem will outperform any PetaLibrary tier). The underlying hardware is configured to be suitable for direct compute access, and as such is accessible from all compute environments (Alpine, Blanca, Open OnDemand). 


**Archive Tier**

PetaLibrary archive allocations are configured to maximize data integrity over performance. As such, archive allocations are not accessible from compute nodes, but can be accessed interactively on login nodes.

**Active + Archive Tier**

The PetaLibrary active+archive is a means of automatically replicating your data across our active and archive PetaLibrary tiers. Rather than purchase an active and archive allocation and synchronizing data between the two, you can purchase the active+archive tier, and we will manage the replication for you. With this type of allocation, you have full access to the active tier only, and we manage replicating data to the archive tier for you.

**Archive + DR**

Although the Archive allocation tier is robust, all current allocations within this tier are hosted in a single location. In the event of a disaster at this location, we cannot guarantee the safety of this data. To add an extra layer of protection, we offer the Archive + DR (Disaster Recovery) tier. The Archive + DR tier offers the same benefits as the Archive tier in addition to a monthly backup of the data to an offsite location. The backup process and any needed restoration efforts will be managed entirely by CURC staff.

**IDEA - Add a graphic hear highlighting the differences between Active, Archive, Active + Archive**

<div style="display: flex; align-items:center; padding:1em; border-top: dashed 1px; border-bottom: dashed 1px; " >

  <img alt="Read the Docs Logo" src="img/RTD_Logo_Dark.svg" style="width:150px; margin-right:15px; background-color:white; border-radius:5px; padding:5px;"> 

  <p style="margin-bottom:0;" > You can find an in-depth comparision of the different tiers in the [PetaLibrary Allocation Tiers Documentation](https://curc.readthedocs.io/en/latest/petalibrary/index.html) </p>

</div>


---

## Data Recovery

A PetaLibrary active or archive allocation is a single copy of your data that is not backed up. PetaLibrary can be a component of a good backup strategy, but for data that cannot be replaced, an active or archive PetaLibrary should not be the only copy. While the PetaLibrary active+archive and archive+DR (DR=disaster recovery) tiers provide additional redundancy for your data, please remember (as stated in the [PetaLibrary Terms of Service](https://www.colorado.edu/rc/resources/petalibrary/tos)) it is the sole responsiblity of the allocation onwer to ensure critical data are backed up. 


> [!TIP]
> If you have any questions or concerns about data recovery related to PetaLibrary, please reach out to CURC's User Support team. They would be happy to discuss your specific data storage needs over email or in a virtual consultation. You can reach the User Support Team through the [online support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).

---

## PetaLibrary Roles

**Owner**

Every PetaLibrary allocation must have one owner. The owner is permitted to make any changes to the allocation, including file permissions, allocation size, and destruction of data. The owner is responsible for furnishing payment for PetaLibrary expenses. An optional technical contact and billing contact may also be defined. These contacts are treated as delegates of the allocation owner for normal or regular operation.

**Technical Contact**

A PetaLibrary allocation may have one or more technical contacts. Technical contacts are largely identical to billing contacts, in that they may speak on behalf of the owner of an allocation, short of designating a new owner.

**Billing Contact**

PetaLibrary allocation may have one or more billing contacts. A billing contact may speak on behalf of the owner of an allocation, making any change that an owner would, except designating a new owner.


> [!IMPORTANT]
> In most cases, PetaLibrary allocation owners are principal investigators (PI). While student researchers may be listed as the owner of an allocation, we strongly advise against this. We recommend listing the student's advisor or the PI of the associated research project as the allocation's owner. 

--- 

## ✏ Knowledge Check

**TODO - Create a quick questionnaire for PL Allocations**

<div style="width:45%; margin: 15px 2.5%; float:left; ">

![A cartoon graphic representing an HPC Cluster's hardware](img/HPC_Clusters.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

Which of the following research tasks are suitable for an HPC cluster, like Alpine or Blanca? (Select all that apply)

<!-- 
data-solution-button="off" 
data-text-failed="Not Quite. Make sure to select ALL of the suitable research tasks." 
data-text-solved="Correct! Both of the suitable research tasks have been selected!"-->
[[X]] Training a deep learning neural network model using a large dataset (Gigabytes to Terabytes) 
[[ ]] Creating a spreadsheet to calculate the average weight and height of 30 penguins 
[[X]] Running a computational fluid dynamics (CFD) simulation of airflow over an airplane's wing 
[[ ]] Hosting an interactive website for visualizing historical weather data 
<script>
//Expected format for @input is a numeric array
// [0,0,0,1] 
let response = ""
let check = 0

//  Neutral Net
if (@input[0] == "1") {
  response += "<b>Training a deep learning... - Correct!</b> <br> Training a deep learning neural network requires a massive amount of simultaneous computations and a lots of memory capacity to handle the model and dataset, making it a classic HPC workflow. <br> <br>"
  check+=1
} 

//Spreadsheet 
if (@input[1]  == "1") {
  response += "<b> Creating a spreadsheet... - Not Quite.</b>  <br> This task is a simple, sequential calculation that requires minimal resources and is easily handled by a standard personal computer. It does not benefit from or require the parallel power of a cluster. <br> <br>"
  check-=1
} 

// CFD Simulation
if (@input[2] == "1") {
  response += "<b> Running a computational fluid ... - Correct!</b> <br> Simulations often require coordinated, parallel computation across many CPU cores (and GPUs) in order to complete within a reasonable timeframe. <br> <br>"
  check+=1
} 

// Hosting a website
if (@input[3] == "1") {
  response += "<b> Hosting an interactive website... - Not Quite.</b> <br> While visualizing large datasets can be a great HPC workflow, CURC does not support web servers. Research workflows that require always-on services (like web servers) need to be setup in the cloud or on a non-CURC cluster.  <br> <br>"
  check-=1
} 

document.getElementById("hpc_question_responses").innerHTML = response

if(check == 2){
  let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"NEW_USER_SCORES", question:"HPC_CLUSTERS", value: currentDate.toLocaleString()})
  send.lia("true")
} else { send.lia("")}

//Note - the wait line is required for lia to properly use the send option to the quiz

"LIA: wait"
</script>

<div id="hpc_question_responses"></div>

</div>

<div style="clear:both"></div>


---

## Conclusion

<script hidden>
 let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_VISITS", question:"CONCLUSION", value: currentDate.toLocaleString()})
  "LIA: wait"
</script>


**Congratulations! You have completed CURC's PetaLibrary Training!**

If you have specific questions about CURC resources, please fill out our [support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).

To learn more about Research Computing, con`sider:

* Reading the [Online Documentation](https://curc.readthedocs.io/en/latest/getting_started/navigating_docs.html).
* Attending a [Workshop Training Session or Consult Hours](https://curc.readthedocs.io/en/latest/getting_started/trainings_and_consults/index.html).

--- 


<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; margin: 0 auto;" >

Please let us know how useful you found this online training: 

  [(2)] Very useful
  [(1)] Somewhat useful
  [(0)] Neutral 
  [(-1)] Not so useful
  [(-2)] Not very useful
<script>
  let choices = @input;
  for (const [key, value] of Object.entries(choices)) {
    if(value === 1){
      sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"REVIEW_SCORE", value: key })
      send.lia("Feedback Sent", [], false)
      break;
    }
  }

</script>

</div>

<br>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; margin: 0 auto;" >

Please provide any comments you would like to share with us on this course: 

[[___ ___ ___ ___]]
<script>
  let feedback= `@input`
  feedback = feedback.replace(/(\r\n|\n|\r)/g, "_"); //Google Script wont accept breaks, must replace with "_" to ensure the data is received/saved.
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"COMMENTS", value: feedback})
  send.lia("Feedback Sent", [], false)
  "LIA: wait"
</script>


</div>

<div style="clear:both"></div>