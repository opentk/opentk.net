---
layout: page
title: Resources
permalink: /resources/
---

<!-- scripts/styles -->
<script src="//code.jquery.com/jquery-1.12.3.js"></script>
<script src="https://cdn.datatables.net/1.10.12/js/jquery.dataTables.min.js"></script>
<style src="https://cdn.datatables.net/1.10.12/css/jquery.dataTables.min.css"></style>
<script>
$(document).ready(
	function(){
		$('#resourceTable').DataTable();
	}
);
</script>

<!-- Content -->
<table id="resourceTable" class="display" cellspacing="0" width="100%">
	<thead>
		<tr>
			<th>Name</th>
			<th>Author</th>
			<th>OpenTK version</th>
			<th>OpenGL version</th>
			<th>Type</th>
		</tr>
	</thead>
	<tfoot>
		<tr>
			<th>Name</th>
			<th>Author</th>
			<th>OpenTK version</th>
			<th>OpenGL version</th>
			<th>Type</th>
		</tr>
	</tfoot>
	<tbody>
		{% for resource in site.data.opentk_resources %}
		<tr>
			<td><a href="{{resource.Location}}">{{resource.Title}}</a></td>
			<td>{{ resource.Author }}</td>
			<td>{{ resource.OpenTK_Version }}</td>
			<td>{{ resource.OpenGL_Version }}</td>
			<td>{{ resource.Type }}</td>
		</tr>
		{% endfor %}
	</tbody>
</table>