function run() {
    var smartSheetID = "TARGET_SHEET_ID";
    var smartSheetToken = "SMARTSHEET_TOKEN";
    var sheet = SpreadsheetApp.openById("GOOGLE_SHEET_ID").getSheets()[0];
    SpreadsheetApp.flush();
    
    var url = "https://api.smartsheet.com/2.0/sheets/" + smartSheetID;
    var response = UrlFetchApp.fetch(
        url, { headers: {Authorization: 'Bearer ' + smartSheetToken}}
    );
    var result = JSON.parse(response.getContentText());
    
    var url = "https://api.smartsheet.com/2.0/sheets/" + smartSheetID + '/rows?ids=';
    
    for(var row in result.rows){
        var id = result.rows[row]["id"];
        var cells = result.rows[row]["cells"];
      
        for(var cell in cells) {
        // insert here additional logic if you want to delete only particular rows rather than everything
            var url = url + id + ',';
        }
           
      }
    
    var url = url.slice(0, -1);
    var url = url + '&ignoreRowsNotFound=true';
    
    Logger.log(url);
    
    var options = {
        method: 'DELETE',
        headers: {Authorization: 'Bearer ' + smartSheetToken}
      };   
       
    var url2 = "https://api.smartsheet.com/2.0/sheets/" + smartSheetID + '/rows';

       
    var data = sheet.getDataRange().getValues();
    
    for (i in data) {
            
       if (data[i][1] != '') {
     
       Logger.log(data[i][1], data[i][8]);
      
       var earliestDate = new Date(data[i][7]);       
       var earliestDate = Utilities.formatDate(earliestDate, SpreadsheetApp.getActive().getSpreadsheetTimeZone(), "yyyy-MM-dd");
      
       var latestDate = new Date(data[i][8]);
       var latestDate = Utilities.formatDate(latestDate, SpreadsheetApp.getActive().getSpreadsheetTimeZone(), "yyyy-MM-dd");
              
       var row = [{
          "toTop": true,
          "cells": [
              {
                "columnId": SMARTSHEET_COLUMN_ID,
                 "value": data[i][1]
              },
              {
                "columnId": SMARTSHEET_COLUMN_ID,
                "value": data[i][12]        
              },
              {
                "columnId": SMARTSHEET_COLUMN_ID,
                "value": data[i][13]        
              },
              {
                "columnId": SMARTSHEET_COLUMN_ID,
                "value": earliestDate
              },
              {
                "columnId": SMARTSHEET_COLUMN_ID,
                "value": latestDate
              }
              
          ]
        }
      ];       
      
     var options2 = {
        method: 'POST',
        contentType: 'application/json',
        payload: JSON.stringify(row),
        headers: {Authorization: 'Bearer ' + smartSheetToken}
      };
    
     //submit a row into Smartsheet
     var response = UrlFetchApp.fetch(
        url2, options2); 
       
    }
    }
    
    // Delete the the rows collected earlier
    var response = UrlFetchApp.fetch(
      url, options); 
}
