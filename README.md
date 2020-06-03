# cm-data-grab
A piece of javascript to pull cloud manager data into a JSON object for query

* Instructions

1. Navigate to the Cloud manager/Atlas organisation of your choice.
2. Right click and 'Inspect' the page to open the developer console.
3. Paste the code below in to the console

```
// Get all info in org
var myOrgData = null;
var completedRequests = 0;

$.getJSON("https://cloud.mongodb.com/user/shared").done(function(data) {
   var myOrgId = data.currentOrganization.orgId;


   $.getJSON("https://cloud.mongodb.com/orgs/" + myOrgId + "/groups").done(function(data) {
       myOrgData = data; //console.log(data);
       data.forEach((elem,index) => {
           // console.log(elem)
           $.getJSON("https://cloud.mongodb.com/servers/list/" + elem.id + "?mapAutomationAgents=true&mapBackupAgents=true&mapMonitoringAgents=true&mapProcesses=true").done(function(groupdata) {
               // console.log(groupdata);
               data[index].processInfo = groupdata;
               completedRequests +=1;
               console.log("Completed " + completedRequests + "/" + data.length)
               if (completedRequests >= data.length) {
                   console.log("All Requests Completed")
               }
           }).fail(function() {
               console.log("FAILED GroupInfo!!!")
           })
       });
   }).fail(function() {
       console.log("FAILED org Info!!!")
   })
})
```
4. Once you receive the message 'All requests completed' paste the below in to the console
```
copy(myOrgData)
```
5. You now have the organisation data in a JSON object in your clipboard.  Feel free to interogate it by inserting it in to MongoDB, or your data processor of choice.
