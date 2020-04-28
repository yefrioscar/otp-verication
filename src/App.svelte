<script>
  import { fly } from "svelte/transition";
  import Button from "./components/Button.svelte";

  let email = "yefrioscar9814@gmail.com";
  let code;
  let codeSend;
  let timerCount;
  let isLoading;
  let isVerify;

  const send = async e => {
    try {
      isLoading = true;
      let response = await fetch("https://sls-cus-dev-api.azurewebsites.net/api/auth/send", {
        method: "POST",
        body: JSON.stringify({ email }),
        headers: {
          "Content-Type": "application/json"
        }
      });

      isLoading = false;

      let { expirationTime } = await response.json();

      timer(expirationTime);

      codeSend = true;
    } catch (error) {
      console.log(error);
    }
  };

  const verify = async e => {
    try {
      isLoading = true;
/*       let response = await fetch("http://localhost:4001/api/auth/verify", {
 */      let response = await fetch("https://sls-cus-dev-api.azurewebsites.net/api/auth/verify", {
        method: "POST",
        body: JSON.stringify({ email, code }),
        headers: {
          "Content-Type": "application/json"
        }
	  });
	  
      isLoading = false;
		isVerify = true;

      console.log(response);

      let s = await response.json();

      console.log(s);
      console.log("Success verify");
    } catch (error) {
      console.log(error);
    }
  };

  const timer = async seconds => {
    // Set the date we're counting down to
    var d1 = new Date();
    d1.setSeconds(d1.getSeconds() + (seconds - 60));
    var countDown = d1.getTime();

    // Update the count down every 1 second
    var x = setInterval(function() {
      // Get today's date and time
      var now = new Date().getTime();

      // Find the distance between now and the count down date
      var distance = countDown - now;

      // Time calculations for days, hours, minutes and seconds
      var days = Math.floor(distance / (1000 * 60 * 60 * 24));
      var hours = Math.floor(
        (distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      );
      var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      var seconds = Math.floor((distance % (1000 * 60)) / 1000);

      // Output the result in an element with id="demo"
      /* 		timerCount = days + "d " + hours + "h "+ minutes + "m " + seconds + "s ";
       */ timerCount = `${getPart(days, "d")}${getPart(hours, "h")}${getPart(
        minutes,
        "m"
      )}${getPart(seconds, " segundos")}`;

      // If the count down is over, write some text
      if (distance < 0) {
        clearInterval(x);
        timerCount = "EXPIRED";
      }
    }, 1000);

    function getPart(number, type) {
      return number ? `${number}${type} ` : "";
    }
  };

  const reset = e => {
    isVerify = false;
  }
</script>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
	width: 300px;
	padding-top: 2rem;
	padding-left: 1rem;
	padding-right: 1rem;
  }

  	@media screen and (max-width: 600px) {
		main {
			width: 100%;
		}
	}	

  input {
    border: none;
    background-color: rgb(230, 230, 230);
    border-radius: 5px;
    height: 40px;
    width: 100%;
    margin-bottom: 10px;
    outline: none;
    padding-left: 10px;
    font-size: 16px;
    color: rgb(104, 104, 104);
  }

  input::placeholder {
    color: rgb(192, 192, 192);
  }

  input:focus {
    border-color: red;
  }

  span {
    display: block;
    width: 100%;
    color: rgb(156, 156, 156);
  }

  p {
	  margin-bottom: .5rem;
  }

  h1 {
	  margin-bottom: 1rem;
	  font-weight: bold;
	  text-align: start;
	  align-self: flex-start;
  }
</style>

<main>
	<h1>Verification Email</h1>

	{#if !isVerify}
		<input
			bind:value={email}
			placeholder="Email"
			/>
		{#if codeSend}
			<input bind:value={code} placeholder="Code verification"  />
			<Button on:click={verify} {isLoading} text="Verify" />
		{:else}
			<Button on:click={send} {isLoading} text="Send" />
		{/if}
		{#if timerCount}
			{#if timerCount === 'EXPIRED'}
			<span >El codigo ha expirado</span>
			{:else}
			<span >El codigo vence en {timerCount}</span>
			{/if}
		{/if}
		 
	{:else}
		<p > Thansk for verify your email!</p>
		<Button on:click={reset} text="Repear process" />
	{/if}

</main>
