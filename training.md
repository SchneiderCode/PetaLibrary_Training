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

  <p style="margin-bottom:0;" > This tutorial provides a quick overview of CURC's PetaLibrary service. For more detailed information, please see [PetaLibrary's online documentation](https://curc.readthedocs.io/en/latest/petalibrary/index.html) </p>

</div>

---

## Creating an Allocation

Step 1: Choose the allocation tier that is right for you.

Step 2: Request a new PetaLibrary allocation (links to Docusign form)

Step 3: Learn to use PetaLibrary via this documentation.

TODO - Convert into an infographic / diagram. 

---

## Allocation Tiers 

**TODO - Add a graphic here highlighting the differences between Active, Archive, Active + Archive, Acrhive+DR. Not necessarily as in-depth as the one provided in CURC's documentation. More of an infographic that provides a very high level overview. Maybe something like "Tier -> Common Use Case"**

        {{1}}
*************
Active Tier 

PetaLibrary active allocations are the most performant PetaLibrary tier (please note that performant is a relative term – our parallel scratch filesystem will outperform any PetaLibrary tier). The underlying hardware is configured to be suitable for direct compute access, and as such is accessible from all compute environments (Alpine, Blanca, Open OnDemand). 

*************

        {{2}}
*************
Archive Tier

PetaLibrary archive allocations are configured to maximize data integrity over performance. As such, archive allocations are not accessible from compute nodes, but can be accessed interactively on login nodes.
*************

        {{3}}
*************
Active + Archive Tier

The PetaLibrary active+archive is a means of automatically replicating your data across our active and archive PetaLibrary tiers. Rather than purchase an active and archive allocation and synchronizing data between the two, you can purchase the active+archive tier, and we will manage the replication for you. With this type of allocation, you have full access to the active tier only, and we manage replicating data to the archive tier for you.
*************

        {{4}}
*************
Archive + DR

Although the Archive allocation tier is robust, all current allocations within this tier are hosted in a single location. In the event of a disaster at this location, we cannot guarantee the safety of this data. To add an extra layer of protection, we offer the Archive + DR (Disaster Recovery) tier. The Archive + DR tier offers the same benefits as the Archive tier in addition to a monthly backup of the data to an offsite location. The backup process and any needed restoration efforts will be managed entirely by CURC staff.
*************

      {{5}}
*************
<div style="display: flex; align-items:center; padding:1em; border-top: dashed 1px; border-bottom: dashed 1px; " >

  <img alt="Read the Docs Logo" src="img/RTD_Logo_Dark.svg" style="width:150px; margin-right:15px; background-color:white; border-radius:5px; padding:5px;"> 

  <p style="margin-bottom:0;" > You can find an in-depth comparision of the different tiers in the [PetaLibrary Allocation Tiers Documentation](https://curc.readthedocs.io/en/latest/petalibrary/index.html) </p>

</div>
*************

---

### ✏ Knowledge Check

Question Here

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

### ✏ Knowledge Check

Question Here

---

## Data Recovery

A PetaLibrary active or archive allocation is a single copy of your data that is not backed up. PetaLibrary can be a component of a good backup strategy, but for data that cannot be replaced, an active or archive PetaLibrary should not be the only copy. While the PetaLibrary active+archive and archive+DR (DR=disaster recovery) tiers provide additional redundancy for your data, please remember it is the sole responsiblity of the allocation onwer to ensure critical data are backed up ([PetaLibrary Terms of Service](https://www.colorado.edu/rc/resources/petalibrary/tos)). 


> [!TIP]
> If you have any questions or concerns about data recovery related to PetaLibrary, please reach out to CURC's User Support team through the [online support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).

---

### ✏ Knowledge Check

<div style="width:45%; margin: 15px 2.5%; float:left; ">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >


Who is responsible for ensuring data stored in a PetaLibrary Allocation is backed-up?  

<!-- 
data-solution-button="off" 
data-text-failed="Not Quite." 
data-text-solved="Correct! "-->
[(X)] The allocation's owner
[( )] Research Computing
[( )] No one, data backups are automatically handled by the system. 
<script>
//Expected format for @input is an index - 0 (first entry), 1 (second entry), so on..
let response = ""

//  Owner - Correct Answer
if ("@input" === "0") {
  response = "<b>Allocation Owner - Correct!</b> <br> Explanation <br> <br>"
} 

//RC 
if ("@input" === "1") {
  response = "<b> Research Computing... - Not Quite.</b>  <br> Explanation <br> <br>"
} 

// No One
if ("@input" === "2") {
  response = "<b> No one... - Not Quite.</b> <br> Explanation <br> <br>"
} 

document.getElementById("data_backup_question_responses").innerHTML = response

if("@input" === "0"){
  let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"BACKUP_RESPONSIBILITY", value: currentDate.toLocaleString()})
  send.lia("true")
} else { send.lia("")}

//Note - the wait line is required for lia to properly use the send option to the quiz

"LIA: wait"
</script>

<div id="data_backup_question_responses"></div>

</div>

<div style="clear:both"></div>


//Note - the wait line is required for lia to properly use the send option to the quiz

"LIA: wait"
</script>

<div id="pl_owner_question_responses"></div>

</div>

<div style="clear:both"></div>

---

## Accessing an Allocation

Add a brief overview of data transfer options

[https://curc.readthedocs.io/en/latest/petalibrary/data_transfer.html#data-transfer](https://curc.readthedocs.io/en/latest/petalibrary/data_transfer.html#data-transfer)

<section>
### Adding Collaborators

Discuss the process of requesting new users to be added to an allocation (i.e. group change tickets)

</section>

--- 

### ✏ Knowledge Check

<div style="width:45%; margin: 15px 2.5%; float:left; ">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

Which PetaLibrary Role has authorization to approve access to an allocation's files and directories?
<!-- 
data-solution-button="off" 
data-text-failed="Not Quite. Make sure to select ALL of the suitable PetaLibrary Roles." 
data-text-solved="Correct!"-->
[[X]] Technical Contact
[[X]] Owner
[[X]] Billing Contact 
[[ ]] Primary Investigator or Research Advisor
<script>
//Expected format for @input is a numeric array
// [0,0,0,1] 
let response = ""
let check = 0

//  Neutral Net
if (@input[0] == "1") {
  response += "<b>Technical Contact - Correct!</b> <br> Technical contacts are authorized to request most changes on behalf of the PetaLibrary owner. <br> <br>"
  check+=1
} 

//Spreadsheet 
if (@input[1]  == "1") {
  response += "<b> Owner - Correct! </b>  <br> The Owner is authorized to request changes to an allocation. They are also the only one authorized to retire an allocation or selecting a new owner. <br> <br>"
  check+=1
} 

// CFD Simulation
if (@input[2] == "1") {
  response += "<b> Billing Contact - Correct!</b> <br> Billing contacts are authorized to request most changes on behalf of the PetaLibrary owner. <br> <br>"
  check+=1
} 

// Hosting a website
if (@input[3] == "1") {
  response += "<b> PI or Advisor - Not Quite.</b> <br> A PI or advisor for a student researcher are not necessarily authorized to make PetaLibrary changes. They may only requests changes if they are one of the approved PetaLibrary Roles (Technical Contact, Billing Contact, or Owner). This is why we recommend listing the PI or advisor as the owner of an allocation. <br> <br>"
  check-=1
} 

document.getElementById("pl_add_collaborators_question_responses").innerHTML = response

if(check == 3){
  let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"PL_ADD_COLLABORATORS", value: currentDate.toLocaleString()})
  send.lia("true")
} else { send.lia("")}

//Note - the wait line is required for lia to properly use the send option to the quiz

"LIA: wait"
</script>

<div id="pl_add_collaborators_question_responses"></div>

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