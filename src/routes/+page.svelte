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
        <td>✅ Uses the default browser <code>beforeunload</code> event</td>
      </tr>
      <tr>
        <td>Browser Back / Forward into external link</td>
        <td>✅ Uses the default browser <code>beforeunload</code> event</td>
      </tr>
      <tr>
        <td><a href="https://svelte.dev/" rel="noreferrer">External link in THIS tab</a></td>
        <td>
          <p>❌ Should trigger the default browser <code>beforeunload</code> event (alert / dialog)</p>
          <p>The <a href="https://kit.svelte.dev/docs/modules#$app-navigation-beforenavigate" target="_blank">beforeNavigate documentation</a> states that:</p>
          <p><em>"calling cancel will trigger the native browser unload confirmation dialog. In these cases, navigation.willUnload is true."</em></p>
          <p>Here we are given <code>navigation.willUnload: true</code>, but the behaviour doesn't reflect the docs</p>
        </td>
      </tr>
      <tr>
        <td><a href="https://svelte.dev/" rel="noreferrer" target="_blank">External link in NEW tab</a></td>
        <td>✅ The link opens in a new tab, it doesn't trigger navigation</td>
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
