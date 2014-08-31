
// Form.php//

<!doctype html>
<html>
	<head>
		<title> LVCC Computer Programming 2nd Year </title>
		<link rel="stylesheet" href="style.css" />
		<script type="text/javascript" src= "FormValidation.js"></script>
	</head>
	<body>
		<div id="wrap">
		<h2> LVCC Computer Programming <br> 
			 Second Year <br> 
			 S.Y.2014-2015 </b></center></h2>
			<table border="2" width = 100%>
					<tr>
					<th>Last Name</th>
					<th>First Name</th>
					<th>Middle Name</th>
					<th>Birthday</th>
					<th>Age</th>
					<th>Gender</th>
					<th>Email Address</th>
					<th>Contact Number</th>

					<?php	
						$cp2student = file("studentlist.csv");
						foreach($cp2student as $student) :
							list($last_name, $first_name, $middle_name, $birthday, $age, $gender, $email_address, $contact_number) = explode(',',$student);
					?>
							<tr>
								<td> <?php echo $last_name; ?> </td>
								<td> <?php echo $first_name; ?> </td>
								<td> <?php echo $middle_name; ?> </td>
								<td> <?php echo $birthday; ?> </td>
								<td> <?php echo $age; ?>
								<td> <?php echo $gender; ?> </td>
								<td> <?php echo $email_address; ?> </td>
								<td> <?php echo $contact_number; ?> </td>
							</tr>
						<?php endforeach; ?>
				</table>
		</div>
	</body>
		<body>
			<form id= "MyForm" action = "submit.php" method = "POST">
				
				<tr>
					<td> Last Name: <span class="red"></span></td>
					<td>
						<input type = "text" size = "20" id="last_name" name= "last_name"/>&nbsp;&nbsp;
						<span id="last_nameError" class="red"></span>
					</td>
				</tr><br>
				<tr>
					<td> First Name: <span class="red"></span></td>
					<td> 
						<input type = "text" size = "20" id="first_name" name= "first_name"/>&nbsp;&nbsp;
						<span id="first_nameError" class="red"></span>
					</td>
				</tr><br>
				<tr>
					<td> Middle Name: <span class="red"></span></td>
					<td>
						<input type = "text" size = "20" id="middle_name" name= "middle_name"/>&nbsp;&nbsp;
						<span id="middle_nameError" class="red"></span>
					</td>
				</tr><br>
				<tr> 
					<td> Birthday: <span class="red"></span></td>
					<td>
						<input type = "text" size = "20" id="birthday" name = "birthday"/>&nbsp;&nbsp;
						<span id="birthdayError" class="red"></span>
					</td>
				</tr><br>
				<tr>
					<td> Age: <span class="red"></span></td>
					<td>
						
						<select id="age" name = "age"/>&nbsp;&nbsp;
						<span id="ageError" class="red"></span>
							<option selected="" value="Default">(choose)</option>  
							<?php for($i=100;$i>0;$i--):?>
								<option	value = "<?php echo $i; ?>"> <?php echo $i; ?> </option>
							<?php endfor; ?>
						</select>
					</td>
				</tr><br>
				<tr>
					<td> Gender: <span class="red"></span></td>
					<td>
						<input type="radio" name="gender" value="Male" />Male
        		    	<input type="radio" name="gender" value="Female" />Female</td>
        		 		<span id="genderError" class="red"></span>&nbsp;
        		</td>
        		</tr><br>   
        		<tr>
        			<td> Email Address: <span class="red"></span></td>
        			<td>
        				<input type = "text" size = "25" id= "email_address" name = "email_address"/>&nbsp;&nbsp;
        				<span id="email_addressError" class="red"></span>&nbsp;</td></tr>
        		</td>
        		</tr><br>
        		<tr>
        			<td> Contact Number: <span class="red"></span></td>
        			<td>
        				<input type = "text" size = "22" id= "contact_number" name = "contact_number"/>&nbsp;&nbsp;
        				<span id="contact_numberError" class="red"></span>&nbsp;</td></tr>
        			</td>
        		</tr><br>
        		<tr>
        			<td>&nbsp;</td>
        			<td>
        				<input type = "submit" value = "SUBMIT"/>&nbsp;&nbsp; 
        				<input type="reset" value="CLEAR" id="reset"/></td>
					</td>&nbsp;</td>
				</tr>		
				</table>
			</form>			
		</body>		
</html>
//submit.php//
<?php
	$last_name = $_POST['last_name'];
	$first_name=$_POST['first_name'];
	$middle_name = $_POST['middle_name'];
	$birthday = $_POST['birthday'];
	$age = $_POST['age'];
	$gender = $_POST['gender'];
	$email_address= $_POST['email_address'];
	$contact_number=$_POST['contact_number'];

	$cp2student = array($last_name, $first_name, $middle_name, $birthday, $age, $gender, $email_address, $contact_number);
	$student = fopen("studentlist.csv", "a");
	fputcsv($student, $cp2student);
	fclose($student);
	header ("Location:Form.php");
?>

// FormValidation.js//

window.onload = init;
function init() {
   document.getElementById("MyForm").onsubmit = validateForm;
   document.getElementById("reset").onclick = clearDisplay;
   document.getElementById("last_name").focus();
}
function validateForm() {
   return (
           isNotEmpty("last_name", "Please enter your last name!")
        && isNotNumeric("first_name", "Please enter your first name in letters only, not in numeric!")
        && isLengthMinMax("middle_name", "Please enter your middle name! Minimum of 5 letters and maximum of 25 letters...", 5, 25)
        && isNotEmpty("birthday", "Please enter your birthday!")
        && isNotEmpty("age", "Please select your age !")
        && isChecked("gender", "Please choose your gender!")
        && isValidemail_address("email_address", "Enter a valid email address!")
        && isNumeric("contact_number", "Please enter a valid contact number! Without spacing and special character!")
        );
}
function isNotEmpty(inputId, errorMsg) {
   var inputElement = document.getElementById(inputId);
   var errorElement = document.getElementById(inputId + "Error");
   var inputValue = inputElement.value.trim();
   var isValid = (inputValue.length !== 0);  // boolean
   showMessage(isValid, inputElement, errorMsg, errorElement);
   return isValid;
}
function showMessage(isValid, inputElement, errorMsg, errorElement) {
   if (!isValid) {
      if (errorElement !== null) {
         errorElement.innerHTML = errorMsg;
      } else {
         alert(errorMsg);
      }
      if (inputElement !== null) {
         inputElement.className = "error";
         inputElement.focus();
      }
   } else {
      if (errorElement !== null) {
         errorElement.innerHTML = "";
      }
      if (inputElement !== null) {
         inputElement.className = "";
      }
   }
}
function isNotNumeric(inputId, errorMsg) {
   var inputElement = document.getElementById(inputId);
   var errorElement = document.getElementById(inputId + "Error");
   var inputValue = inputElement.value.trim();
   var isValid = (inputValue.search(/^[a-zA-Z]+$/) !== -1);
   showMessage(isValid, inputElement, errorMsg, errorElement);
   return isValid;
}
function isNumeric(inputId, errorMsg) {
   var inputElement = document.getElementById(inputId);
   var errorElement = document.getElementById(inputId + "Error");
   var inputValue = inputElement.value.trim();
   var isValid = (inputValue.search(/^[0-9]+$/) !== -1);
   showMessage(isValid, inputElement, errorMsg, errorElement);
   return isValid;
}
function isLengthMinMax(inputId, errorMsg, minLength, maxLength) {
   var inputElement = document.getElementById(inputId);
   var errorElement = document.getElementById(inputId + "Error");
   var inputValue = inputElement.value.trim();
   var isValid = (inputValue.length >= minLength) && (inputValue.length <= maxLength);
   showMessage(isValid, inputElement, errorMsg, errorElement);
   return isValid;
}
function isValidemail_address(inputId, errorMsg) {
   var inputElement = document.getElementById(inputId);
   var errorElement = document.getElementById(inputId + "Error");
   var inputValue = inputElement.value;
   var atPos = inputValue.indexOf("@");
   var dotPos = inputValue.lastIndexOf(".");
   var isValid = (atPos > 0) && (dotPos > atPos + 1) && (inputValue.length > dotPos + 2);
   showMessage(isValid, inputElement, errorMsg, errorElement);
   return isValid;
}
function isChecked(inputName, errorMsg) {
   var inputElements = document.getElementsByName(inputName);
   var errorElement = document.getElementById(inputName + "Error");
   var isChecked = false;
   for (var i = 0; i < inputElements.length; i++) {
      if (inputElements[i].checked) {
         isChecked = true;  // found one element checked
         break;
      }
   }
   showMessage(isChecked, null, errorMsg, errorElement);
   return isChecked;
}
function clearDisplay() {
   var elms = document.getElementsByTagName("*");  // all tags
   for (var i = 0; i < elms.length; i++) {
      if ((elms[i].id).match(/Error$/)) {  // no endsWith() in JS?
         elms[i].innerHTML = "";
      }
      if (elms[i].className === "error") {  // assume only one class
         elms[i].className = "";
      }
   }
   document.getElementById("last_name").focus();
}

//style.css


.red {
   color: #FF0000;
   font-size: 20px;
}
 
/* for the error input text fields */
input.error {
   border: 1px red inset;
   padding: 2px;
}
body {
	font-family: Berlin Sans FB, sans-serif;
	font-size: 13pt;
	color: blue;
	background-color: lightgreen;
}
#wrap {
	width: 100%;
	margin: 0 auto;
	text-align: center;
}
h2 {
	font-family: Niagara Solid, sans-serif;
	font-size: 50pt;
	font-weight: bold;
	margin: .2em;
	line-height: 1em;
	color: blue;
}
table, td, tr, th {
	border: 0;
	margin: 1em 0;
}
table {
	border: 3px solid blue;
	text-align: left;
}
th {
	font-family: Poor Richard;
	font-size: 1.2em;
	font-weight: bold;
	color: #000000;
	padding: 10px;
	background-color: #ffff00;
}
tr {
	background-color: #ffffff;
}
form {
	font-family: Times New Roman;
	font-size: 1.2em;
	font-weight: bold;
	
	font-color: black;
}
input[type="text"] {
	background-color: white;
	font-weight: bold;
}
select[name="age"] {
	background-color: #008000;
	font-weight: bold;
}
select[name="gender"] {
	background-color: #008000;
	font-weight: bold;
}
input[type="submit"] {
	background-color: #008000;
	font-weight: bold;
}
input[type="reset"] {
	background-color: #008000;
	font-weight: bold;
}

// studentlist.csv //

Aguilar,Reagan,Honrales,1/25/1994,12,Male,ezekiel_2501@yahoo.com,0935-522-9627
Aquino,Joshua Genesis,Dela Isla,4/6/1993,13,Male,josh_gen06@yahoo.com,0933-249-9197
Bacala,Leah,Zorilla,2/28/1989,17,Female,leahbacala@gmail.com,0909-354-6336
Capili,Bryan,Sante,11/9/1992,13,Male,bryancapili34@yahoo.com,0977-168-7383
Catua,Krisdel Joy,Morillo,7/30/1990,16,Female,krisdeljoycatua@gmail.com,0906-689-6399
Coronel,Christopher,Guanzon,7/31/1996,10,Male,christopher_1127@yahoo.com,0927-287-5928

