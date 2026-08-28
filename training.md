<!--

script:  https://cdn.jsdelivr.net/npm/mermaid@11.17.2/dist/mermaid.min.js


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

mermaid.initialize({ startOnLoad: false,  flowchart: { wrappingWidth: 400, subGraphTitleMargin: {"top": 15,"bottom": 0} } });


@end

@mermaid: @mermaid_(@uid,```@0```)

@mermaid_
<script run-once="true" modify="false" style="display:block; background: white">
async function draw () {
    const graphDefinition = `@1`;
    const { svg } = await mermaid.render('graphDiv_@0', graphDefinition);
    send.lia("HTML: "+svg);
    send.lia("LIA: stop")
};

draw()
"LIA: wait"
</script>
@end

@mermaid_eval: @mermaid_eval_(@uid)

@mermaid_eval_
<script>
async function draw () {
    const graphDefinition = `@input`;
    const { svg } = await mermaid.render('graphDiv_@0', graphDefinition);
    console.html(svg);
    send.lia("LIA: stop")
};

draw()
"LIA: wait"
</script>
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

## Setting Up an Allocation 

```mermaid @mermaid

flowchart TD

    A@{shape: text, label: "The first step in creating an allocation is determining which of the four PetaLibrary tiers provides the right performance and backup level for your research. Each type will be explained in greater detail in the next section 'Allocation Tiers'." }

    B@{shape: text, label: "Once you have determined the appropriate tier, your next step will be to submit an allocation request using the <a href='https://powerforms.docusign.net/189e8917-6143-431e-ad8a-663bddf6a345?env=na2&acct=088d5d64-ef4d-40bb-acf2-480eabbf546d&accountId=088d5d64-ef4d-40bb-acf2-480eabbf546d'>Docusign Form</a>. Be aware that allocation's are purchased in TB units, with a minimum size of 1TB and a maximum size of 200TB." }

    C@{shape: text, label: "After the request has been processed and your allocation has been provisioned, you will want to start using your allocation. The 'Access an Allocation' section of this tutorial covers how access is granted to an allocation's data storage and tools for transferring data to and from the allocation. " }

    style A_Plot fill:#d4e6f1,stroke:#2874a6,stroke-width:2px,color:#154360
    style A text-align:left
    style B_Plot fill:#d5f5e3,stroke:#239b56,stroke-width:2px,color:#145a32
    style B text-align:left
    style C_Plot fill:#fcf3cf,stroke:#d4ac0d,stroke-width:2px,color:#7d6608
    style C text-align:left


   subgraph A_Plot["Step 1: Choose a Tier"]
      direction LR
      A
    end

    subgraph B_Plot["Step 2: Submit a Request"]
      direction LR
      B
    end

    subgraph C_Plot["Step 3: Access the Allocation"]
      direction LR
      C
    end

    A_Plot --> B_Plot
    B_Plot --> C_Plot
    
```


---

### PetaLibrary Roles

Before getting into the details of the different PetaLibrary tiers or various data transfer tools, it is important to understand the different roles that govern access to the allocation. Those roles are the owner, the technical contact, and the billing contact. 

**Owner**

Every PetaLibrary allocation must have one owner. The owner is permitted to make any changes to the allocation, including file permissions, allocation size, and deletion of data. 
The owner is also responsible for furnishing payment for PetaLibrary expenses. An optional technical contact and billing contact may also be defined.
These contacts are treated as delegates of the allocation owner for normal or regular operation.  

**Billing & Technical Contacts**

PetaLibrary allocation may have one or more billing contacts and/or technical contacts. They may speak on behalf of the owner of an allocation, making any change that an owner would, except designating a new owner. The key difference between the two is that only Billing Contacts are included messaging related to billing (e.g. annual invoices for allocation rewewals) 

> [!IMPORTANT]
> In most cases, PetaLibrary allocation owners are principal investigators (PI). While student researchers may be listed as the owner of an allocation, we strongly advise against this. If a student owner graduates, future changes to the allocation can become complicated. 
> We recommend the following assignees for each role 
> * *Owner*: PI, Faculty Advisor, or Researcher
> * *Technical Contact*: Lab Manager, Lead Graduate Student 
> * *Billing Contact*: Department Accountant or SpeedType manager 


---

### ✏ Knowledge Check

<div style="width:45%; margin: 15px 2.5%; float:left; ">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>


<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

Which of the following actions can **ONLY** be performed by the Owner of a PetaLibrary allocation?

[( )] Adding new users to the allocation's access group
[( )] Requesting a storage quota increase
[(X)] Designating a new Owner
[( )] Modifying file permissions
<script>
let response = ""

if ("@input" === "2") {
  response = "<b>Correct!</b> <br> While Technical and Billing contacts can speak on behalf of the owner for normal operations, only the current Owner can designate a new Owner. <br> <br>"
} else if("@input" === "0" || "@input" === "1" || "@input" === "3"){
  response = "<b>Not Quite.</b> <br> Technical and Billing contacts act as delegates and are authorized to perform standard operations like modifying permissions or requesting quota increases. <br> <br>"
}

document.getElementById("roles_question_responses").innerHTML = response

if ("@input" === "2") {
  let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"ROLES_OWNERSHIP", value: currentDate.toLocaleString()})
  send.lia("true")
} else {
  send.lia("")
}

"LIA: wait"
</script>

<div id="roles_question_responses"></div>

</div>
<div style="clear:both"></div>


---

## Choosing an Allocation Tier 

| Allocation Tier  | Compute Node Access? | Common Use Case |
| :---  | :---: | :--- |
| **Active^1^** | ✅ (YES)| Data actively being computed on via Alpine or Blanca. |
| **Archive^2^** | ❌ (NO)| Long-term cold storage for completed projects or raw datasets. |
| **Active + Archive^3^** | ✅ (YES)| Working data that requires an automatic copy for safety. |
| **Archive + DR^4^** | ❌ (NO)| Irreplaceable cold data requiring off-site disaster recovery backups. |

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

The PetaLibrary active+archive is a means of automatically replicating your data across our active and archive PetaLibrary tiers. With this type of allocation, you have full access to the active tier only, and we manage replicating data to the archive tier for you.
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

  <p style="margin-bottom:0;" > You can find an in-depth comparision of the different tiers, and current pricing, in the [PetaLibrary Allocation Tiers Documentation](https://curc.readthedocs.io/en/latest/petalibrary/index.html) </p>

</div>

*************

---

### Data Recovery

A PetaLibrary active or archive allocation is a single copy of your data that is not backed up. PetaLibrary can be a component of a good backup strategy, but for data that cannot be replaced, an active or archive PetaLibrary should not be the only copy. While the PetaLibrary active+archive and archive+DR (DR=disaster recovery) tiers provide additional redundancy for your data, please remember it is the sole responsiblity of the allocation onwer to ensure critical data are backed up ([PetaLibrary Terms of Service](https://www.colorado.edu/rc/resources/petalibrary/tos)). 


> [!TIP]
> If you have any questions or concerns about data recovery related to PetaLibrary, please reach out to CURC's User Support team through the [online support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).


---

### ✏ Knowledge Check

<div style="width:45%; margin: 15px 2.5%; float:left; ">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>


<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

<!-- 
data-solution-button="off" 
data-text-failed="Not Quite." 
data-text-solved="Correct! "-->
Which PetaLibrary tier is required if you need your data to be directly accessible from the Alpine or Blanca compute nodes?

[(X)] Active (or Active + Archive)
[( )] Archive
[( )] Archive + DR
[( )] All tiers allow compute node access
<script>
let response = ""
if ("@input" === "0") {
  response = "<b>Correct!</b> <br> Active allocations are configured for direct compute access. Archive allocations maximize data integrity over performance and can only be accessed interactively on login nodes. <br> <br>"
} else if("@input" === "1" || "@input" === "2" || "@input" === "3"){
  response = "<b>Not Quite.</b> <br> Remember that archive-based tiers prioritize data integrity over performance and cannot be mounted directly to compute nodes. <br> <br>"
}

document.getElementById("tier_question_responses").innerHTML = response

if ("@input" === "0") {
  let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"TIER_SELECTION", value: currentDate.toLocaleString()})
  send.lia("true")
} else {
  send.lia("")
}

"LIA: wait"
</script>

<div id="tier_question_responses"></div>

--- 

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



---

## Accessing an Allocation

Access to a PetaLibrary allocation is securely managed using a **Linux Access Group**. 

When an allocation is created, it is assigned to either an existing RC group or a newly created one. To grant a new researcher access to the files:
1. The user must first create a Research Computing account.
2. The PetaLibrary **Owner** or **Technical Contact** must submit a support ticket requesting the user be added to the Linux group.
3. In the request, specify the name of the PetaLibrary allocation. This is especially important for users who are listed as an Owner or Technical Contact on multiple allocations. 

> [!NOTE]
> Individual file permissions within the allocation are still controlled by standard Linux commands (e.g. `chmod`). It is up to the allocation's users to manage file permissions, but the User Support Team can provide guidance and assistance via the [Support Request Form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form). 


### Data Transfer Tools
Depending on your workflow, there are several ways to move data in and out of your allocation:

* **Globus:** The recommended method for large data transfers. You can also create a *Globus Guest Collection* to securely share PetaLibrary data with external collaborators who do not have CURC accounts.
* **Local Mounting (SMB/CIFS):** You can mount your PetaLibrary allocation directly to your local Mac, Windows, or Linux workstation while connected to the campus VPN. 
* **Rclone / SFTP:** Ideal for backing up local data directly to your allocation via the command line.

<div style="display: flex; align-items:center; padding:1em; border-top: dashed 1px; border-bottom: dashed 1px; " >

  <img alt="Read the Docs Logo" src="img/RTD_Logo_Dark.svg" style="width:150px; margin-right:15px; background-color:white; border-radius:5px; padding:5px;"> 

  <p style="margin-bottom:0;" > You can find additional information on these and other data transfer tools in the [PetaLibrary Data Transfers Documentation](https://curc.readthedocs.io/en/latest/petalibrary/data_transfer.html) </p>

</div>

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