<a name="top"></a>

<p align="center"> 
  <a href="https://www.linkedin.com/in/clarence-fong" target="_blank">
    <img width="50" height="50" alt="LinkedIn" src="https://github.com/user-attachments/assets/7ab6e12b-ca8a-4aa6-8f10-79ba89d485b5" />
  </a>
  <a href="mailto:abc1230940@gmail.com">
    <img width="50" height="50" alt="Gmail" src="https://github.com/user-attachments/assets/4e0491ce-239c-413c-b433-74a5ff48f231" />
  </a>
  <a href="https://www.instagram.com/cyberbrexel?igsh=MXNxeWJid2VxZWxxaw%3D%3D&utm_source=qr" target="_blank">
    <img width="50" height="50" alt="Instagram Old" src="https://github.com/user-attachments/assets/62e4672b-d424-4489-a204-c301040905a3" />
  </a>
  <a href="https://discordapp.com/users/cyberbrexel" target="_blank">
    <img width="50" height="50" alt="Discord" src="https://github.com/user-attachments/assets/f76173ca-fad3-4390-bca1-5c2305bc748e" />
  </a>
  <a href="https://www.reddit.com/user/abc1230940/" target="_blank">
    <img width="50" height="50" alt="Reddit" src="https://github.com/user-attachments/assets/6e9bd985-dfa3-4349-b966-4cf49362bd61" />
  </a>
</p> <br>


<h2 align="center"> CyberDefenders Write-up - KrakenKeylogger </h2>
<h2 id="scenario"> Scenario </h2>
<p> An employee at a large company was assigned a task with a two-day deadline. Realizing that he could not complete the task in that timeframe, he sought help from someone else. After one day, he received a notification from that person who informed him that he had managed to finish the assignment and sent it to the employee as a test. However, the person also sent a message to the employee stating that if he wanted the completed assignment, he would have to pay $160. </p>
<p> The helper's demand for payment revealed that he was a threat actor. The company's digital forensics team was called in to investigate and identify the attacker, determine the extent of the attack, and assess potential data breaches. The team must analyze the employee's computer and communication logs to prevent similar attacks in the future. </p>
<p align="right">(<a href="#top">Back to Top</a>)</p>


<h2 id="tools-used"> Tools Used </h2>
<ol>
  <li> DB Browser for SQLite </li>
  <li> VirusTotal </li>
</ol>
<p align="right">(<a href="#top">Back to Top</a>)</p>


<h2 id="questions"> Questions </h2>
<p> <strong> 1. What is the the web messaging app the employee used to talk to the attacker? </strong> </p>
<p> In order to identify the messaging app, we can navigate to <strong>\Users\OMEN\AppData\Local\Microsoft\Windows\Notifications</strong> and locate the wpndatabase.db file, which stores the Windows Push Notification if a message was received. We opened the database with DB Browser for SQLite and navigated to <strong>Browse Data -> Table: Notification</strong>. </p>
<img width="1865" height="993" alt="Screenshot 2026-06-23 181911" src="https://github.com/user-attachments/assets/70db3acf-2d4b-445f-81f9-620509a11dc3" />
<p> The most interesting row was the one with the group column Notifications, so I clicked the Payload for more details. </p>
<img width="1565" height="356" alt="Screenshot 2026-06-23 182121" src="https://github.com/user-attachments/assets/1a44878b-a033-421b-aefc-2274c6600330" />
<p> Based on the notification, there was a message from the social media <strong>Telegram</strong> send from a user Nawaf and the content was "📎 our project templet test.zip,pass:@1122d". </p>
<br>
<p> <strong> 2. What is the password for the protected ZIP file sent by the attacker to the employee? </strong> </p>
<p> Based on the message received, the password of "our project templet test.zip" was <strong>@1122d</strong>. </p>
<br>
<p> <strong> 3. What domain did the attacker use to download the second stage of the malware? </strong> </p>
<p> It was predicted that our project templet test.zip will be downloaded to "/Users/OWEN/Downloads/our project templet test.zip" after clicking the attachment of the message. </p>
<img width="640" height="212" alt="Screenshot 2026-06-23 184020" src="https://github.com/user-attachments/assets/7de68488-696d-49a7-8d00-71c611f37ddd" />
<p> We navigated to the extracted folder "project templet test". </p> 
<img width="623" height="100" alt="Screenshot 2026-06-23 184117" src="https://github.com/user-attachments/assets/3af2a49e-5fad-497a-84ad-7d89ca3c81f0" />
<p> The lnk file templet was suspicious. Therefore I uploaded to VirusTotal for static analysis. </p>
<img width="1816" height="791" alt="Screenshot 2026-06-23 184429" src="https://github.com/user-attachments/assets/e7b0f23a-5d92-4711-827f-13cbf1042f8c" />
<p> 42 out of 62 vendors flagged the lnk file as malicious. It was believed that Owen clicked the malcious lnk file. We can look at the Code Insights to learn how the malware functions. </p>
<img width="1745" height="111" alt="Screenshot 2026-06-23 184606" src="https://github.com/user-attachments/assets/ec929a78-7fab-4ea0-89e1-0c65276853d8" />
<p> When the malicious lnk file was clicked, powershell.exe was spawned to decode the obfuscated malicious script and downloaded the second payload from attacker server. The script also executed the second payload and then deleted it. </p>
<img width="867" height="213" alt="Screenshot 2026-06-23 185141" src="https://github.com/user-attachments/assets/86316650-1adc-47dd-b586-92aa181249b0" />
<p> when we navigated to the Relations section and looked at the contacted URLs, we can identify that the lnk file would reach out to a domain <strong>masherofmasters[.]cyou/</strong> to download se1.hta, which was believed as the second payload. </p>
<br>
<p> <strong> 4. What is the name of the command that the attacker injected using one of the installed LOLAPPS on the machine to achieve persistence? </strong> </p>

<p align="right">(<a href="#top">Back to Top</a>)</p>


<h2 id="reference"> Reference </h2>
<p align="right">(<a href="#top">Back to Top</a>)</p>
