<div class="section" id="restreaming-external-sources">
<h2>Restreaming External Sources<a class="headerlink" href="#restreaming-external-sources" title="Permalink to this headline">¶</a></h2>
<p>Ant Media Server operates with different streaming flows. As well as accepting and creating streaming media, it also has the capability of the pull live streams from external sources. Such as; live TV streams, IP camera streams or other forms of live streams(RTSP, HLS, TS, FLV etc.).  We will first show how to pull live streams with Web Interface and then we will give technical details for developers.</p>
<div class="section" id="pull-live-streams-with-web-interface">
<h3>Pull Live Streams with Web Interface<a class="headerlink" href="#pull-live-streams-with-web-interface" title="Permalink to this headline">¶</a></h3>
<p>First, log in to the management panel. Click New Live Stream -&gt; Stream Source. Define stream name and URL.</p>
<img alt="_images/restreaming-define-stream-parameters.png" src="_images/restreaming-define-stream-parameters.png">
<p>Then, Ant Media Server starts to pull streams.</p>
<img alt="_images/restreaming-stream-pulling.png" src="_images/restreaming-stream-pulling.png">
<p>Now, you can watch it.</p>
<img alt="_images/restreaming-watch-pulled-stream.png" src="_images/restreaming-watch-pulled-stream.png">
</div>
<div class="section" id="define-settings">
<h3>2. Define Settings<a class="headerlink" href="#define-settings" title="Permalink to this headline">¶</a></h3>
<div class="code Java highlight-default notranslate"><div class="highlight"><pre><span></span><span class="n">streamFetcherManager</span><span class="o">.</span><span class="n">setRestartStreamFetcherPeriod</span><span class="p">(</span><span class="n">appSettings</span><span class="o">.</span><span class="n">getRestartStreamFetcherPeriod</span><span class="p">());</span>
<span class="n">streamFetcherManager</span><span class="o">.</span><span class="n">setStreamCheckerInterval</span><span class="p">(</span><span class="s2">"seconds"</span><span class="p">));</span>
</pre></div>
</div>
<p>A developer can define restart parameter. This is stored in the settings file as  ” settings.streamFetcherRestartPeriod” (in seconds) by using setRestartStreamFetcherPeriod() method. If this parameter set as 0, stream fetcher will never restart in other words never save the fetched stream. For example, define this parameter to “1800” to save fetched stream as recorded video in every 30 minutes. Also, make sure that “Enable MP4 Recording” option enabled in the management panel or “settings.mp4MuxingEnabled ” parameter in the setting file set to true to save the video.</p>
<p>Another parameter is StreamCheckerInterval. This defines the time interval for StreamFetcherManager should check all fetching processes whether they continue to fetch properly or not. The default parameter is 10 seconds. The checkStreamFetchersStatus() method performs these controls.</p>
</div>
<div class="section" id="start-streaming">
<h3>3. Start Streaming<a class="headerlink" href="#start-streaming" title="Permalink to this headline">¶</a></h3>
<div class="code Java highlight-default notranslate"><div class="highlight"><pre><span></span><span class="n">streamFetcherManager</span><span class="o">.</span><span class="n">startStreams</span><span class="p">(</span><span class="n">streams</span><span class="p">);</span>
</pre></div>
</div>
<p>The parameter “streams” is a list (List&lt;Broadcast&gt; streams) of broadcast objects to be fetched.</p>
</div>
<div class="section" id="customize">
<h3>4. Customize<a class="headerlink" href="#customize" title="Permalink to this headline">¶</a></h3>
<p>You can also customize the required parameters in StreamFetcher class according to your project’s needs and requirements.</p>
<p><strong>connection timeout:</strong>  (defined as “timeout” in ms) Connection timeout interval during connection establishment to the stream source in the first phase.</p>
<p><strong>packet receiving timeout:</strong> (defined as “PACKET_RECEIVED_INTERVAL_TIMEOUT” in ms) Checks incoming packets timestamps and defines stream is alive or not.</p>
<p><strong>restart after disconnection:</strong> (defined as “restartStream” as boolean) If defined as true, tries to reconnect stream source immediately after disconnected.</p>
<p><strong>buffer time:</strong> (defined as “bufferTime” in ms) Stream Fetcher first buffers received packets then restreams for smooth streaming but of course it creates a small delay.</p>
</div>
</div>