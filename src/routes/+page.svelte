<script>
	import { beforeNavigate } from '$app/navigation';

	beforeNavigate((navigation) => {
		console.log('beforeNavigate', navigation);
    navigation.cancel();
	});
</script>

<div>
  <p>Note: Before testing these, refresh the page each time and interact with it (click the bg).</p>
  <p>Why interact with the page? It's a <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event#security">browser security thing</a>, unrelated to Svelte</p>
  <table>
    <thead>
      <tr>
        <th>Link</th>
        <th>Notes</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Browser Refresh</td>
        <td>Uses the default browser <code>beforeunload</code> event. Which is good</td>
      </tr>
      <tr>
        <td>Browser Back / Forward into external link</td>
        <td>Uses the default browser <code>beforeunload</code> event. Which is good</td>
      </tr>
      <tr>
        <td><a href="https://svelte.dev/" rel="noreferrer">External link in THIS tab</a></td>
        <td>I'd expect this to also trigger the default browser <code>beforeunload</code> event. In practice it's identical to browser back / forward.</td>
      </tr>
      <tr>
        <td><a href="https://svelte.dev/" rel="noreferrer" target="_blank">External link in NEW tab</a></td>
        <td>Since the link should open in a new tab, it shouldn't trigger navigation all. As it stands we're told that <code>willUnload: true</code> which isn't right.</td>
      </tr>
    </tbody>
  </table>
  <p>Conclusion:</p>
  <ul>
    <li>Links that open in a new tab should not trigger <code>beforeNavigate</code></li>
    <li>
      It would be useful to be notified whether <code>navigation.cancel()</code> will trigger the browser <code>beforeunload</code> behaviour.
      <br/>
      That way we could handle it better:
      <pre>{
`const causedDefaultUnloadEvent = navigation.cancel();
if(!causedDefaultUnloadEvent){
  showNativeUI = true;
}
`}
      </pre>
    </li>
  </ul>
    
</div>

<style>
  div {
    font-family: sans-serif;
  }
  table {
    border: solid 1px lightgray;
    border-collapse: collapse;
    border-spacing: 0;
  }
  table thead th {
    background-color: lightgrey;
    border: solid 1px lightgray;
    padding: 10px;
    text-align: left;
  }
  table tbody td {
    border: solid 1px lightgray;
    padding: 10px;
    text-shadow: 1px 1px 1px #fff;
  }
</style>
