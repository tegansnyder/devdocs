---
layout: default
group: config-guide 
subgroup: CLI
title: Manage the cache
menu_title: Manage the cache
menu_node: 
menu_order: 5
github_link: config-guide/cli/config-cli-subcommands-cache.md
redirect_from: /guides/v1.0/config-guide/cli/config-cli-subcommands-cache.html
---


#### Contents

*	<a href="#config-cli-subcommands-cache-clean">Overview of cache types</a>
* <a href="#config-cli-before">First steps</a>
*	<a href="#config-cli-subcommands-cache-status">View the cache status</a>
*	<a href="#config-cli-subcommands-cache-en">Enable or disable the cache</a> 
*	<a href="#config-cli-subcommands-cache-clean">Clean and flush cache types</a>

<h2 id="config-cli-subcommands-cache-clean">Overview of cache types</h2>
Magento 2 has the following cache types:

<table>
  <tbody>
    <tr>
      <th>Cache type "friendly" name</th>
      <th>Cache type code name</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><p>Configuration</p></td>
      <td><p>config</p></td>
      <td><p>Magento collects configuration from all modules, merges it, and saves the merged result to the cache. This cache also contains store-specific settings stored in the file system and database. </p>
	<p>Clean or flush this cache type after modifying configuration files.</p></td>
    </tr>
    <tr>
      <td><p>Layout</p></td>
      <td><p>layout</p></td>
      <td><p>Compiled page layouts (that is, the layout components from all components).</p>
	<p>Clean or flush this cache type after modifying layout files.</p></td>
    </tr>
    <tr>
      <td><p>Block HTML output</p></td>
      <td><p>block_html</p></td>
      <td><p>HTML page fragments per block.</p>
	<p>Clean or flush this cache type after modifying the view layer.</p></td>
    </tr>
    <tr>
      <td><p>Collections data</p></td>
      <td><p>collections</p></td>
      <td><p>Results of database queries.</p>
      	<p>If necessary, Magento cleans up this cache automatically, but third-party developers can put any data in any segment of the cache. </p>
      	<p>Clean or flush this cache type if your custom module uses logic that results in cache entries that Magento cannot clean.</p></td>
    </tr>
    <tr>
      <td><p>DDL</p></td>
      <td><p>db_ddl</p></td>
      <td><p>Database schema. </p>
      	<p>If necessary, Magento cleans up this cache automatically, but third-party developers can put any data in any segment of the cache. </p>
      	<p>Clean or flush this cache type after you make custom changes to the database schema. (In other words, updates that Magento does not make itself.)</p>
      	<p>One way to update the database schema automatically is using the <a href="{{ site.gdeurl }}install-gde/install/install-cli-subcommands-db.html">magento setup:db-schema:upgrade</a> command.</p></td>
    </tr>
    <tr>
      <td><p>Entity attribute value (EAV)</p></td>
      <td><p>eav</p></td>
      <td><p>Metadata related to EAV attributes (for example, store labels, links to related PHP code, attribute rendering, search settings, and so on).</p>
        <p>You should not typically need to clean or flush this cache type.</p></td>
    </tr>
    <tr>
      <td><p>Page cache</p></td>
      <td><p>full_page</p></td>
      <td><p>Generated HTML pages. </p>
      	<p>If necessary, Magento cleans up this cache automatically, but third-party developers can put any data in any segment of the cache. </p>
      	<p>Clean or flush this cache type after modifying code level that affects HTML output. It’s recommended to keep this cache enabled to because caching HTML improves performance significantly.</p></td>
    </tr>
    <tr>
      <td><p>Translations</p></td>
      <td><p>translate</p></td>
      <td><p>Merged translations from all modules.</p></td>
    </tr>
    <tr>
      <td><p>Integration configuration</p></td>
      <td><p>config_integration</p></td>
      <td><p>Compiled integrations.</p>
      	<p>Clean or flush this cache after changing or adding integrations.</p></td>
    </tr>
    <tr>
      <td><p>Integration API configuration</p></td>
      <td><p>config_integration_api</p></td>
      <td><p>Compiled integration APIs.</p></td>
    </tr>
    <tr>
      <td><p>Web services configuration</p></td>
      <td><p>config_webservice</p></td>
      <td><p>Web API structure.</p></td>
    </tr>

  </tbody>
</table>

<h2 id="config-cli-before">First steps</h2>
{% include install/first-steps-cli.html %}
In addition to the command arguments discussed here, see <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands.html#config-cli-subcommands-common">Common arguments</a>.

<h2 id="config-cli-subcommands-cache-status">View the cache status</h2>
To view the status of the cache, enter

	magento cache:status 

<!-- where `--bootstrap=` is a URL-encoded associative array of Magento <a href="{{ site.gdeurl }}config-guide/bootstrap/magento-how-to-set.html#config-bootparam-overview">application bootstrap parameters</a> and values. -->

A sample follows:

                        config: 1
                        layout: 1
                    block_html: 1
                   collections: 1
                        db_ddl: 1
                           eav: 1
                     full_page: 1
                     translate: 1
            config_integration: 1
        config_integration_api: 1
             config_webservice: 1

<h2 id="config-cli-subcommands-cache-en">Enable or disable cache types</h2>
This command enables you to enable or disable all cache types or only the ones you specify. Disabling cache types is useful during development because you see the results of your changes without having to flush the cache; however, disabling cache types has an adverse affect on performance.

Command options:

	magento cache:enable [--all] [type] ... [type]
	magento cache:disable [--all] [type] ... [type] 

where

`--all` enables or disables all cache types at the same time.

`type` is a space-separated list of cache types.

<!-- `--bootstrap=` is a URL-encoded associative array of Magento <a href="{{ site.gdeurl }}config-guide/bootstrap/magento-how-to-set.html#config-bootparam-overview">application bootstrap parameters</a> and values. -->

To list cache types and their status, enter

	magento cache:status

For example, to disable the full page cache and the DDL cache, enter 

	magento cache:disable db_ddl full_page

Sample result:

	Changed cache status:	
                        db_ddl: 1 -> 0
                     full_page: 1 -> 0

<div class="bs-callout bs-callout-info" id="info">
<span class="glyphicon-class">
  <p>Enabling a cache type automatically clears that cache type.</p></span>
</div> 

<h2 id="config-cli-subcommands-cache-clean">Clean and flush cache types</h2>
To purge out-of-date items from the cache, you can *clean* or *flush* cache types:

*	Cleaning a cache type deletes all items from enabled cache types only.

	Disabled cache types are not cleaned.

*	Flushing a cache type purges the cache storage.

	All cache types that use the specified storage are flushed.

Command usage:

	magento cache:clean [--all] [type] ... [type]
	magento cache:flush [--all] [type] ... [type]

where

`--all` cleans or flushes all cache types at the same time.

`type` is a space-separated list of cache types.

<!-- `--bootstrap=` is a URL-encoded associative array of Magento <a href="{{ site.gdeurl }}config-guide/bootstrap/magento-how-to-set.html#config-bootparam-overview">application bootstrap parameters</a> and values.
 -->
For example, to flush all cache types, enter

	magento cache:flush --all

Sample result:

	Flushed cache types:
	config
	layout
	block_html
	collections
	db_ddl
	eav
	full_page
	translate
	config_integration
	config_integration_api
	config_webservice


#### Related topics

* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-index.html">Manage the indexers</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-log.html">Clean the logs</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-cron.html">Configure and run cron</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-compiler-multi.html">Multi-tenant compiler</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-compiler-single.html">Single-tenant compiler</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-depen.html">Dependency reports</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-i18n.html">Translation dictionaries and language packages</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-static-view.html">Deploy static view files</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-less-sass.html">Create LESS from CSS</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-test.html">Run tests</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-layout-xml.html">Convert layout XML files</a>
* <a href="{{ site.gdeurl }}config-guide/cli/config-cli-subcommands-perf-data.html">Generate data for performance testing</a>