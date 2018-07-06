<template>
  <div class="hello">
	
	  <div class="container is-fluid">
		  <div class="notification">
			
			
			  <div class="columns">
				  <div class="column">
					  <div class="file">
						  <label class="file-label">
							  <input class="file-input" type="file" name="resume" accept=".json"  @change="processFile($event)">
							  <span class="file-cta">
				      <span class="file-icon">
				        <i class="fas fa-upload"></i>
				      </span>
				      <span class="file-label">
				        Choose a file…
				      </span>
				    </span>
						  </label>
					  </div>
					  
					  <button type="button" class="button is-success is-outlined" @click="createOriginalData">Create</button>
					  
					  <div v-if="typeFile !== ''">You uploaded <b>{{typeFile}}</b> original file</div>
				  </div>
				  <div class="column">
					  <button type="button" class="button is-success is-outlined" @click="downloadFiles">Download result</button>
				  </div>
				  <div class="column">
					  <button type="button" class="button is-outlined" @click="createJsonText">Create JSON Code</button>
				  </div>
			  </div>
		  </div>
	  </div>
	
	  <!--<div class="container is-fluid">
			  <div class="columns table is-bordered">
				  <div class="column">
					  <div class="notification">
					    <h2 class="subtitle">Original</h2>
					  </div>
				  </div>
				  <div class="column">
					  <div class="notification">
					    <h2 class="subtitle">Your translate</h2>
					  </div>
				  </div>
				  <div class="column">
					  <div class="notification">
					    <h2 class="subtitle">Result</h2>
					  </div>
				  </div>
			  </div>
	  </div>-->
	
	  <div class="container is-fluid">
		  <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
			  <thead>
			    <tr>
				    <th style="width:10%">Original</th>
				    <th style="width:40%">Your translate</th>
				    <th style="width:50%">Result</th>
			    </tr>
			  </thead>
			  <tbody id="tbody">
			  
			  </tbody>
			  
			  
			 <!-- <div class="column">
					  <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth" id="original-text">Original</table>
			  </div>
			  <div class="column">
					  <div id="translate-text"></div>
			  </div>
			  <div class="column">
					  <table id="result-text"></table>
			  </div>-->
		  </table>
	  </div>
	
	  <div class="container is-fluid">
		  <h3>Result:</h3> <p>Wrap line?<input type="checkbox" @click="wrapToggle"/></p>
		  <code v-bind:style="{ whiteSpace: wrap }">{{translateText}}</code>
	  </div>
  </div>
</template>

<script>
export default {
  name: 'JsonTranslateHelper',
  data: function () {
    return {
	    originalText: {},
	    translateText: {},
	    originalElement: '',
	    translateElement: '',
	    resultElement: '',
	    tableBody:'',
	    typeFile:'', // site/faq
	    wrap: 'normal'
    }
  },
	mounted: function (  ) {
		this.originalElement = document.querySelector('#original-text');
		this.translateElement = document.querySelector('#translate-text');
		this.resultElement = document.querySelector('#result-text');
		this.tableBody = document.querySelector('#tbody');
	},
	methods: {
		wrapToggle: function ( e ) {
			this.wrap = e.currentTarget.checked ? 'pre-wrap' : 'normal'
		},
		createJsonText: function (  ) {
			this.translateText = '[{';
			
			var i = 0;
			var text;
			var count = Object.keys(this.originalText).length;
			for ( var key in this.originalText )
			{
				text = '';
				text = this.tableBody.querySelector( 'tr.i'+i+' textarea' ).value;
				if ( text !== '' ) console.log(text);
				this.translateText += '"'+key+'": "'+text;
				
				if ( i+1 < count  )
				{
					this.translateText+='",\n';
				}
				
				i++;
			}
			
			
			this.translateText += '}]';
			window.scrollTo(0,document.body.scrollHeight);
		},
		downloadFiles: function() {
			
			this.translateText = '[{';
			
			var i = 0;
			var text;
			for ( var key in this.originalText )
			{
				text = '';
				text = this.tableBody.querySelector( 'tr.i'+i+' textarea' ).value;
				if ( text !== '' ) console.log(text);
				this.translateText += '"'+key+'": "'+text+'",';
				i++;
			}
			
			
			this.translateText += '}]';
			
			
			this.download(this.translateText, this.typeFile+'-translate.json', 'application/json');
		},
		processFile: function(file) {
			let reader = new FileReader();
			let self = this;
			
			
			reader.onload = (e) => {
				if ( eval(e.target.result)[0][1] === undefined )
				{
					this.typeFile = 'lang';
					this.originalText = eval(e.target.result)[0];
				}
				else
				{
					this.typeFile = 'faq';
					this.originalText = eval(e.target.result)[0];
				}
				this.tableBody.innerText = '';
				
				this.createOriginalData();
			}
			reader.onerror = (stuff) => {
				console.log("error", stuff);
				console.log (stuff.getMessage())
			}
			reader.readAsText(event.target.files[0]);
		},
		createOriginalData: function()
		{
			console.log('create',this.originalText);
			
			var i = 0;
			for ( var key in this.originalText )
			{
				if ( this.typeFile === 'lang' )
				{
					this.insertElementsLang(key, this.originalText[key],i);
					this.createElementsLang(key, this.originalText[key],i);
				}
				else if ( this.typeFile === 'faq' ) {
					console.log(key, this.originalText[key]);
				}
				i++;
			}
		},
		insertElementsLang: function( title, text, index )
		{
			let parentTREl = document.createElement( 'tr' );
			let textEl = document.createElement( 'td' );
			let textE2 = document.createElement( 'td' );
			let textE3 = document.createElement( 'td' );
			
			textEl.innerText = text;
			parentTREl.classList.add('i'+index);
			textE2.classList.add('t');
			textE3.classList.add('r');
			
			parentTREl.appendChild( textEl );
			parentTREl.appendChild( textE2 );
			parentTREl.appendChild( textE3 );
			
			this.tableBody.appendChild( parentTREl );
		},
		createElementsLang: function( title, text, index )
		{
			let parentTDT = document.querySelector( 'tr.i'+index+' td.t' );
			let parentTDR = document.querySelector( 'tr.i'+index+' td.r' );
			let translate = document.createElement( 'textarea' );
			let result = document.createElement( 'div' );
			
			translate.addEventListener( 'input', (e) => {
				result.innerHTML = e.currentTarget.value;
			});
			
			translate.className = 'textarea';
			result.className = ' is-small';
			
			parentTDT.appendChild( translate );
			parentTDR.appendChild( result );
		},
		download: function(strData, strFileName, strMimeType)
		{
			var D = document,
			    A = arguments,
			    a = D.createElement("a"),
			    d = A[0],
			    n = A[1],
			    t = A[2] || "application/json";
		
			//build download link:
			a.href = "data:" + strMimeType + "charset=utf-8," + escape(strData);
			
			
			if (window.MSBlobBuilder) { // IE10
				var bb = new MSBlobBuilder();
				bb.append(strData);
				return navigator.msSaveBlob(bb, strFileName);
			} /* end if(window.MSBlobBuilder) */
		
		
		
			if ('download' in a) { //FF20, CH19
				a.setAttribute("download", n);
				a.innerHTML = "downloading...";
				D.body.appendChild(a);
				setTimeout(function() {
					var e = D.createEvent("MouseEvents");
					e.initMouseEvent("click", true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
					a.dispatchEvent(e);
					D.body.removeChild(a);
				}, 66);
				return true;
			}; /* end if('download' in a) */
		
		
		
			//do iframe dataURL download: (older W3)
			var f = D.createElement("iframe");
			D.body.appendChild(f);
			f.src = "data:" + (A[2] ? A[2] : "application/octet-stream") + (window.btoa ? ";base64" : "") + "," + (window.btoa ? window.btoa : escape)(strData);
			setTimeout(function() {
				D.body.removeChild(f);
			}, 333);
			return true;
		}
		
	},
	computed:
	{
	
	}
}
</script>

<style>
	
	body{
		font-family: -apple-system, "Helvetica Neue", "Helvetica", "Arial", "PingFang SC", "Hiragino Sans GB", "STHeiti", "Microsoft YaHei", "Microsoft JhengHei", SimSun, sans-serif;
	}
	.bold
	{
		font-weight: bold;
	}
</style>
