node {
   stage('Preparation') { 
      // Get some code from a GitHub repository
      git 'https://github.com/mcgonagle/F5file_iRules.git'
   }
   stage('Testing') {
      //Run the tests
      sh "/usr/local/bin/ansible-lint F5file_add_irules.yml"
      sh "/usr/local/bin/ansible-review F5file_add_irules.yml"
   }
   stage('Build') {
       //Ansible Playbook
       ansiblePlaybook(
         colorized: true, 
         inventory: 'hosts.ini', 
         playbook: 'F5file_add_irules.yml', 
         sudoUser: null,
         extraVars: [
            username: 'admin',
            password: [value: 'adminadmin', hidden: true],
            hosts: 'bigip_hosts'
         ])
      //chatops slack message that ansible run has completed
      slackSend( 
         channel: '#general', 
         color: 'good', 
         message: 'F5file iRules added Successfully', 
         teamDomain: 'uniopsteam', 
         token: 'zkMRYtEXCEG3Q2FlUsS2Hjjv'
         )
   }
   stage('Approval') {
      //Gate the process and require approval
      input 'Proceed?'
   }
   stage('Re-Enable') {
       //Ansible Playbook
       ansiblePlaybook(
         colorized: true, 
         inventory: 'hosts.ini', 
         playbook: 'F5file_remove_irules.yml', 
         sudoUser: null,
         extraVars: [
            username: 'admin',
            password: [value: 'adminadmin', hidden: true],
            hosts: 'bigip_hosts'
         ])
      //chatops slack message that ansible run has completed
      slackSend( 
         channel: '#general', 
         color: 'good', 
         message: 'F5file iRules removed successfully', 
         teamDomain: 'uniopsteam', 
         token: 'zkMRYtEXCEG3Q2FlUsS2Hjjv'
         )
   }
}
