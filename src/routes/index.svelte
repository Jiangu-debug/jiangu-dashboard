<script>
    import supabase from '$lib/db';

    async function logout() {
   	 const { error } = await supabase.auth.signOut();

   	 if (error) alert(error.message); // alert if error
    }

    let timetable = {
	Monday: [
  	{
    	name: "PH",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "PM",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "BI",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "R",
    	period: 1,
    	style: "table-success",
  	},
  	{
    	name: "BM",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "M3",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "BC",
    	period: 2,
    	style: "",
  	},
	],
	Tuesday: [
  	{
    	name: "PJPK",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "M3",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "BI",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "E",
    	period: 1,
    	style: "table-success",
  	},
  	{
    	name: "BM",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "BC",
    	period: 3,
    	style: "",
  	},
  	{
    	name: "PJPK",
    	period: 1,
    	style: "",
  	},
	],
	Wednesday: [
  	{
    	name: "BC",
    	period: 3,
    	style: "",
  	},
  	{
    	name: "PM",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "H",
    	period: 1,
    	style: "table-success",
  	},
  	{
    	name: "PKS",
    	period: 3,
    	style: "",
  	},
  	{
    	name: "BM",
    	period: 2,
    	style: "",
  	},
	],
	Thursday: [
  	{
    	name: "SA",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "PJPK",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "BM",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "A",
    	period: 1,
    	style: "table-success",
  	},
  	{
    	name: "PM",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "BC",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "M3",
    	period: 2,
    	style: "",
  	},
	],
	Friday: [
  	{
    	name: "BI",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "BM",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "T",
    	period: 1,
    	style: "table-success",
  	},
  	{
    	name: "BC",
    	period: 2,
    	style: "",
  	},
  	{
    	name: "PM",
    	period: 1,
    	style: "",
  	},
  	{
    	name: "SA",
    	period: 2,
    	style: "",
  	},
	],
  };

  function addTimeSlot(day){
	  if (day === "Monday"){
		timetable.Monday = [
     ...timetable.Monday,
     { name: "??", period: 1, style: "" }
       ];
	}else if (day === "Tuesday"){
		timetable.Tuesday = [
     ...timetable.Tuesday,
     { name: "??", period: 1, style: "" }
       ];
	}else if (day === "Wednesday"){
		timetable.Wednesday = [
     ...timetable.Wednesday,
     { name: "??", period: 1, style: "" }
       ];
	}else if (day === "Thursday"){
		timetable.Thursday = [
     ...timetable.Thursday,
     { name: "??", period: 1, style: "" }
       ];
	}else if (day === "Friday"){
		timetable.Friday = [
     ...timetable.Friday,
     { name: "??", period: 1, style: "" }
       ];
	}

  }
  	let curDay;
   let curIndex;
   let curName;
   let curPeriod;
   let curStyle;

   function showcurData(day,index,name,period,style){
	curDay = day;
    curIndex = index;
    curName = name;
    curPeriod = period;
    curStyle = style;
   }

function deleteTimeSlot(day,index){
	if (day === "Monday") {
  	timetable.Monday.splice(index, 1);
  	timetable = timetable;
	}
    else if (day === "Tuesday") {
  	timetable.Tuesday.splice(index, 1);
  	timetable = timetable;
	}
	else if (day === "Wednesday") {
  	timetable.Wednesday.splice(index, 1);
  	timetable = timetable;
	}
	else if (day === "Thursday") {
  	timetable.Thursday.splice(index, 1);
  	timetable = timetable;
	}
	else if (day === "Friday") {
  	timetable.Friday.splice(index, 1);
  	timetable = timetable;
	}
	saveEntry() 
}

function setTimeSlot (day,index,newName,newPeriod,newStyle){
	  timetable[day][index].name= newName
      timetable[day][index].period= newPeriod
      timetable[day][index].style= newStyle
	  saveEntry()
	}

// Upsert entry
async function saveEntry() {
  const { error } = await supabase.from("studentEntries").upsert(
    {
      user_id: supabase.auth.user().id,
      timetable: timetable,
    },
    { onConflict: "user_id" }
  );
  if (error) alert(error.message);
}

// Get entries
async function getEntries() {
  const { data, error } = await supabase.from("studentEntries").select();
  if (error) alert(error.message);

  if (data != "") {
    timetable = data[0].timetable;
  }
}

getEntries();

</script>

<table class="table caption-top text-center table-bordered">
    <thead class="table-dark">
      <tr>
        <th scope="col">#</th>
        <th scope="col">1</th>
        <th scope="col">2</th>
        <th scope="col">3</th>
        <th scope="col">4</th>
        <th scope="col">-</th>
        <th scope="col">5</th>
        <th scope="col">6</th>
        <th scope="col">7</th>
        <th scope="col">8</th>
        <th scope="col">9</th>
        <th scope="col">10</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th class="table-dark" scope="row">Monday</th>
       {#each timetable.Monday as timeSlot,index}
         <td colspan={timeSlot.period} class={timeSlot.style}>
         <button type="button" class="btn"  data-bs-toggle="modal" data-bs-target="#EditModal" on:click={() =>
			showcurData(
			"Monday",
			  index,
			  timeSlot.name,
			  timeSlot.period,
			  timeSlot.style
			)}
> {timeSlot.name}  </button>
                       
         </td>
       {/each} 
	   <td><button class="btn" on:click={() => addTimeSlot("Monday")}>+</button></td>
      </tr>
      <tr>
        <th class="table-dark" scope="row">Tuesday</th>
        {#each timetable.Tuesday as timeSlot,index}
        <td colspan={timeSlot.period} class={timeSlot.style}>
        <button type="button" class="btn"  data-bs-toggle="modal" data-bs-target="#EditModal"  on:click={() =>
			showcurData(
			"Tuesday",
			  index,
			  timeSlot.name,
			  timeSlot.period,
			  timeSlot.style
			)}> {timeSlot.name}  </button>
		</td>
                      
      {/each}
	  <td><button class="btn" on:click={() => addTimeSlot("Tuesday")}>+</button></td>
      </tr>
      <tr>
        <th class="table-dark" scope="row">Wednesday</th>
        {#each timetable.Wednesday as timeSlot,index}
        <td colspan={timeSlot.period} class={timeSlot.style}>
        <button type="button" class="btn"  data-bs-toggle="modal" data-bs-target="#EditModal"  on:click={() =>
			showcurData(
			"Wednesday",
			  index,
			  timeSlot.name,
			  timeSlot.period,
			  timeSlot.style
			)}> {timeSlot.name}  </button>
        </td>
      {/each}
	  <td><button class="btn" on:click={() => addTimeSlot("Wednesday")}>+</button></td>
      </tr>
      <tr>
        <th class="table-dark" scope="row">Thursday</th>
        {#each timetable.Thursday as timeSlot,index}
        <td colspan={timeSlot.period} class={timeSlot.style}>
        <button type="button" class="btn"  data-bs-toggle="modal" data-bs-target="#EditModal" on:click={() =>
			showcurData(
			"Thursday",
			  index,
			  timeSlot.name,
			  timeSlot.period,
			  timeSlot.style
			)}> {timeSlot.name}  </button>
       
		
        </td>
      {/each}
	  <td><button class="btn" on:click={() => addTimeSlot("Thursday")}>+</button></td>
      </tr>
      <tr>
        <th class="table-dark" scope="row">Friday</th>
        {#each timetable.Friday as timeSlot,index}
         <td colspan={timeSlot.period} class={timeSlot.style}>
         <button type="button" class="btn"  data-bs-toggle="modal" data-bs-target="#EditModal" on:click={() =>
			showcurData(
			"Friday",
			  index,
			  timeSlot.name,
			  timeSlot.period,
			  timeSlot.style
			)}> {timeSlot.name}  </button>
		 </td>             
		 
       {/each}
	   <td><button class="btn" on:click={() => addTimeSlot("Friday")}>+</button></td>
      </tr>
    </tbody>
  </table>

  
  <!-- Modal -->
  <div class="modal fade" id="EditModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
	<div class="modal-dialog">
	  <div class="modal-content">
		<div class="modal-header">
		  <h5 class="modal-title" id="exampleModalLabel">Edit Time Slot</h5>
		  <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
		</div>
		<div class="modal-body">
			<div class="input-group mb-3">
				<span class="input-group-text" id="name">Name</span>
				<input type="text" class="form-control" bind:value={curName}>
			</div>
			<div class="input-group mb-3">
				<span class="input-group-text" id="period">Period</span>
				<input type="number" class="form-control" bind:value={curPeriod} >
			</div>
			<div class="input-group mb-3">
				<label class="input-group-text" for="inputGroupSelect01">Style</label>
				<select class="form-select" id="inputGroupSelect01" bind:value={curStyle}>
				  <option value="">Default</option>
				  <option value="table-primary">Blue</option>
				  <option value="table-success">Green</option>
				  <option value="table-danger">Red</option>
				  <option value="table-warning">Yellow</option>
				  <option value="table-secondary">Grey</option>
				</select>
			  </div>
		</div>
		<div class="modal-footer">
		  <button type="button" class="btn btn-secondary" >Cancel</button>
		  <button
      	type="button"
      	class="btn btn-danger"
      	data-bs-dismiss="modal"
      	on:click={() => deleteTimeSlot(curDay, curIndex)}>Delete</button>

		 <button type="button" class="btn btn-primary" data-bs-dismiss="modal" on:click={() => setTimeSlot(curDay, curIndex,curName,curPeriod,curStyle)}>Save changes</button>
		</div>
	  </div>
	</div>
  </div>

  <section class="container px-4 py-3 text-center">
    <button class="btn btn-secondary" on:click={logout}>Logout</button>
</section>	

