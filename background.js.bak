	//Set default initial value
	﻿if(!localStorage["count"])	//init only once when the app run for the first time
	{
		localStorage["count"] = 0;  //the total number of urls
 		localStorage["trytimes"] = 0;	//times that redirection happens
 		localStorage["default_redirectUrl"] = "http://203.208.46.145";
 		localStorage["redirectUrl"] = localStorage["default_redirectUrl"];
 		localStorage["default_starttime"] = "8";
 		localStorage["default_endtime"] = "22";
 		localStorage["starttime"]  = localStorage["default_starttime"];
 		localStorage["endtime"] = localStorage["default_endtime"];
 	}
 	 		
	// Called when the url of a tab changes.
	function showPageAction(url, tabId) {
	  if (url.indexOf('chrome://extensions') > -1) // 只在扩展程序页面显示PageActiion图标
	  {
	    // show the page action.
	    chrome.pageAction.show(tabId);
	  }
	};

	function blockWebsite(newUrl, tabId, tab) {
		if(!newUrl) return;
		var num = localStorage["count"];
		var starttime = localStorage["starttime"];
		var endtime = localStorage["endtime"];
		var today=new Date();
		var now=today.getHours();
		for(var i=0; i<parseInt(num); ++i){
			var urli = "url" + i;
			var url = localStorage[urli];
			if(newUrl.indexOf(url)>-1 && now>=starttime && now<=endtime) { 
				//if the website is in the list and the time is less what you set,redirect.
				chrome.tabs.update(tabId, { url: localStorage["redirectUrl"] });
				localStorage["trytimes"] = parseInt(localStorage["trytimes"]) + 1;
				var notification = window.webkitNotifications.createNotification(
					'images/icon-48.png',
					'Attention!',
					'Please Focus on your work.  \n'+ 'You have tried ' + localStorage["trytimes"] + ' times!' 
				);
				notification.show();
				return;
			}
		}
	}

	//响应图标点击事件 open the options page
	function openOptions(){
		var url = "options.html";
		var fullUrl = chrome.extension.getURL(url);		//chrome-extension://your extension id//options.html	// 选项页的位置
		chrome.tabs.getAllInWindow(null, function(tabs){
			for(var i in tabs){
				var tab = tabs[i];
				if(tab.url == fullUrl){
					chrome.tabs.update(tab.id, {selected: true});
					return;
				}
			}
			chrome.tabs.getSelected(null, function(tab){
				chrome.tabs.create({ url: url, index: tab.index+1});
			});
		});
	}


	// Listen for any changes to the URL of any tab.
	// 监控标签页的更新事件
	chrome.tabs.onUpdated.addListener(function(tabId, changeInfo, tab){
		if (changeInfo.status == "loading") 
		{
			var myurl = tab.url;
			showPageAction(myurl, tabId); //show pageAction or not
			blockWebsite(myurl, tabId); //if the website is in the list, block it;
		}
		});
		

	// 监控图标的点击事件，参数是回调函数
	chrome.pageAction.onClicked.addListener(openOptions); 
