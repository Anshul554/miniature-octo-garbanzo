<?php
	session_start();
	if(!isset($_SESSION['id']))
	{
		header("Location:login.php");
	}

?>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
<table>

<?php

	function checkfriend($toid,$sessioid)
	{
	include "config.php";
		$r=mysqli_query($con,"select * from friends where touser='$toid' and fromuser='$sessioid'");
		if($row=mysqli_fetch_array($r))
		{
			
			return $row['status'];
		}
	}
	include "config.php";
	$userid=$_SESSION['id'];
	$re=mysqli_query($con,"select username,id,userpic from regi");// sari values fetch
	while($row=mysqli_fetch_array($re))
	{
		
		if($row['id']==$userid)
		{
			continue; //user skip
		}
		else
		{
		?>
		<tr><td><input type="hidden"  class="toid"><?php echo $row['username'];?></td><td><?php
	$rk=checkfriend($row['id'],$userid);
		if($rk=="0")
		{
			echo "Request Send";
		}
		else if($rk=="1")
		{
			echo "friend";
		}
		else
		{?>
			<a href="#" class="clicks" alt="<?php echo $row['id'];?>">Add As Friend</a>
		<?php }
		?></td></tr>
	<?php }
	}
?>
</table>

<script>
	$(document).ready(function(){
		$(".clicks").click(function(){
			$(this).text("Request Send");
			var toid=$(this).attr("alt");
		
			$.ajax({
				type:"post",
				data:{id:toid},
				url:"addfrnd.php",
				success:function(data){
				
				}
				
			})
		});
		
	});
</script>
