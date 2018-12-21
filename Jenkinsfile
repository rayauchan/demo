pipeline {
  agent any
  stages {
    stage('Stop service') {
      parallel {
        stage('Stop service') {
          steps {
            powershell(script: '$srn = "F06E0BB7C3A3P" $srv = "AdobeARMservice" Write-Host "Stopping $svn service..."  Stop-Service -InputObject $srv         $srv.WaitForStatus(\'Stopped\',\'00:00:15\') $srv = Get-Service -ComputerName $srn -Name $svn        if ($srv.Status -eq "Stopped") { Write-Host "Service has been stopped with success"  } else {         Write-Host "Service has been stopped with failure" -ForegroundColor Red         } 	    }', returnStatus: true, returnStdout: true)
          }
        }
        stage('Copy DLLs') {
          steps {
            bat(script: 'Set src = "c:\\temp\\source" Set des = "c:\\temp\\demo" xcopy src des /y', returnStatus: true, returnStdout: true)
          }
        }
      }
    }
    stage('Start service') {
      steps {
        powershell(script: '  $srn = "F06E0BB7C3A3P"  $srv = "AdobeARMservice"  Write-Host "Restarting $svn service..."   Start-Service -InputObject $srv        $srv.WaitForStatus(\'Running\',\'00:00:30\') $srv = Get-Service -ComputerName $srn -Name $svn        if ($srv.Status -eq "Running") {         Write-Host "Service has been restarted with success"         }         else {         Write-Host "Service has been restarted with failure" -ForegroundColor Red         } 		}', returnStatus: true, returnStdout: true)
      }
    }
    stage('Test') {
      steps {
        powershell(script: 'Test-path "c:\\temp\\demo\\*.dll', returnStatus: true, returnStdout: true)
      }
    }
  }
}