<!---
 license: Licensed to the Apache Software Foundation (ASF) under one
         or more contributor license agreements.  See the NOTICE file
         distributed with this work for additional information
         regarding copyright ownership.  The ASF licenses this file
         to you under the Apache License, Version 2.0 (the
         "License"); you may not use this file except in compliance
         with the License.  You may obtain a copy of the License at

           http://www.apache.org/licenses/LICENSE-2.0

         Unless required by applicable law or agreed to in writing,
         software distributed under the License is distributed on an
         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
         KIND, either express or implied.  See the License for the
         specific language governing permissions and limitations
         under the License.
-->

# Download modifyed file-transfer plugin
cordova plugin add https://github.com/shashikantkumar88/xpointers-android-cordova-plugin-file-transfer.git
File download using modified file and file transfer plugin

Declare id in body tag
             Example-
          <body ng-app="starter" animation="slide-left-right-ios7" id="MainControllerID">

Include jquery.js
Download modified file and file transfer form github.
	
cordova plugin add https://github.com/shashikantkumar88/xpointers-android-cordova-plugin-file.git 

cordova plugin add https://github.com/shashikantkumar88/xpointers-android-cordova-plugin-file-transfer.git

If ExternalStorage.java  file is not available in platforms\android\src\org\apache\cordova\file\ExternalStorage.java then copy from https://github.com/shashikantkumar88/xpointers-android-cordova-plugin-file/blob/master/src/android/ExternalStorage.java
 	
	
Following code include inside any controller -


            $scope.filePath = "";    
            $scope.$on('event:gotAvailableSpace', function(e, jsonResultObj) {
                if (jsonResultObj.freeSpace > 10) {
                    if (jsonResultObj.functionCallType === 'downloadFile') {
                        //downloadFile function call
                        $scope.downloadFile($scope.filePath);
                    }
                    else {
                        alert("Copy file is missing.");
                    }
                } else {
                    alert("Can't download and copy a file.Insufficient space.");
                }
            });
            $scope.checkFreeSpace = function(dirEntry, functionCallType) {
                var str = dirEntry.toURL().toString();
                var dirpath = str.substring(0, str.lastIndexOf('/') + 1);
                if (dirpath.indexOf("file://") === 0) {
                    dirpath = dirpath.substring(7);
                }
                var ftrans = new FileTransfer(dirpath);
                ftrans.megabytesAvailable(dirpath, functionCallType);
            };
            $scope.dowmloadFiles = function() {
                var path = 'ILDIPFUpdateIssue6.pdf';
                functionCallType = "downloadFile";
                path = _DownloadTopicPdfURL + path;
                $scope.dirEntry = "";
                var fileExists = false;
                if (path !== "") {
                    $scope.filePath = path;
                    var remoteFile = path;
                    var localFileName = remoteFile.substring(remoteFile.lastIndexOf('/') + 1);
                    window.requestFileSystem(LocalFileSystem.PERSISTENT, 0, function(fileSystem) {
                        fileSystem.root.getDirectory("IPFDownloads", {create: false}, function(dirEntry) {
                            dirEntry.getFile(localFileName, {create: false, exclusive: false}, function(fileEntry) {
                                var fileSource = fileEntry.toURL();
                                window.open(fileSource, '_system', "location=0,addressbar=0");
                            }, function(error) {
                                if (navigator.network.connection.type === "none" || navigator.network.connection.type === null || navigator.network.connection.type === "unknown") {
                                    alert("No internet connection");
                                }
                                else {
                                    $scope.checkFreeSpace(dirEntry, functionCallType);
                                }
                            });
                        }, function(error) {
                            if (navigator.network.connection.type === "none" || navigator.network.connection.type === null || navigator.network.connection.type === "unknown") {
                               alert("No internet connection");
                            }
                            else {
                                $scope.downloadFile($scope.filePath);
                            }
                        });
                    }, function(error) {
                        alert("Error while reading");
                    });
                } else {
                }
            };
            $scope.downloadFile = function(path) {
                alert("Please wait, download may take few minutes");
                var remoteFile = path;
                var localFileName = remoteFile.substring(remoteFile.lastIndexOf('/') + 1);
                window.requestFileSystem(LocalFileSystem.PERSISTENT, 0, function(fileSystem) {
                    fileSystem.root.getDirectory("IPFDownloads", {create: true}, function(dirEntry) {
                        dirEntry.getFile(localFileName, {create: true, exclusive: false}, function(fileEntry) {
                            var localPath = fileEntry.toURL();
                            if (device.platform === "Android" && localPath.indexOf("file://") === 0) {
                                localPath = localPath.substring(7);
                            }
                            var ft = new FileTransfer();
                            ft.download(remoteFile,
                                    localPath, function(entry) {                    
                                        alert("File downloaded successfully");
                                    }, function(error) {
                                alert("Connectivity has been lost. Please try again later");            
                            });
                        }, function(error) {
                            alert("Get file error.");        
                        });
                    }, function(error) {
                        alert("Get dir error.");    
                    });
                }, function(error) {
                    alert("File System Error");
                });
            };

 
