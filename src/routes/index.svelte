<script lang="ts">
	import groovyBeautify from 'groovy-beautify';

	let source: string = `{
def bumpVersion(String target,    String version_type, Boolean reset =   false) {    def
    versionMap =
    ['major':0, 'minor' : 1, 'patch':   2]
                def versionArray = target.findAll(/\d+\.\d+\.\d+/)[0].tokenize('.')
            try
    {        def   index =     versionMap.get(version_type);
    versionArray[index] =versionArray[index].toInteger() + 1
    if(   reset )
    {
        for(int i=2;i>index;     i--) {
            versionArray[i]    =    0            }        }
    } catch(       Exception e) {        println("Unrecognized version type \\"version_type\\" (should be major, minor or patch)")    }
    return             versionArray.join(                   '.'                       )
    }
println(bumpVersion('1.2.3', 'minor', true))
}`;
	$: target = groovyBeautify(source);
</script>

<main>
	<header>
		<h1>Groovy Beautify</h1>
	</header>
	<section>
		<textarea class="source" bind:value={source} />
		<textarea class="target" value={target} />
	</section>
</main>

<style>
	:global(*) {
		box-sizing: border-box;
	}
	main {
		display: grid;
		grid-template-rows: max-content 1fr;
		width: 100vw;
		height: 100vh;
		padding: 12px;
	}
	section {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 12px;
	}
	textarea {
		width: 100%;
		height: 100%;
	}
</style>
