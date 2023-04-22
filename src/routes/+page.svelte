<script lang="ts">
	import SveltyPicker from 'svelty-picker';
	import { Button } from '@svelteuidev/core';
	import { Plus, Cross1 } from 'radix-icons-svelte';
	import { Container } from '@svelteuidev/core';
    import '../style.scss'

	let days: any[] = [
		{ day: 'Monday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] },
		{ day: 'Tuesday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] },
		{ day: 'Wednesday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] },
		{ day: 'Thursday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] },
		{ day: 'Friday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] },
		{ day: 'Saturday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] },
		{ day: 'Sunday', timeSlots: [{ openTime: '', closeTime: '' ,btnDisable:true}] }
	];
    const addMore = (item:string) => {
        days.forEach((element)=>{
            if(element.day == item){
                element.timeSlots.push({ openTime: '', closeTime: '', btnDisable:false})
            }
            
        })
        days = days
    }

    const onRemove = (item:string,index:number) => {
        days.forEach((element)=>{
            if(element.day == item){
                element.timeSlots.splice(index,1)
            }
            
        })
        days = days
    }
</script>

<Container override={{ dflex: 'center', fd: 'column', my: '80px' }}>
	{#each days as item}
		<Container override={{ dflex: 'center', mt: '10px' }}>
			<Container override={{ mx: '30px', w: '100px', textAlign: 'center' }}>
				{item.day}
			</Container>
			<Container override={{d:'flex',fw:'wrap',w:'350px',pr:'0px'}}>
				{#each item.timeSlots as hours, index}
                    <Container override={{mb:'8px'}}>
                        <SveltyPicker
                            placeholder="open"
                            mode="time"
                            format="hh:ii"
                            bind:value={hours.openTime}
                            inputClasses="inputTime"
                        />
                        <SveltyPicker
                            placeholder="close"
                            mode="time"
                            format="hh:ii"
                            bind:value={hours.closeTime}
                            inputClasses="inputTime"
                        />
                    </Container>
                    <Container override={{ dflex: 'center' }}>
                        <Button override={{mx:'6px'}} on:click="{() => addMore(item.day)}">
                            <Plus />
                        </Button>
                        <Button override={{mx:'6px',bgColor:'Silver'}} on:click="{() => onRemove(item.day,index)}" disabled={hours.btnDisable}>
                            <Cross1 />
                        </Button>
                    </Container>
				{/each}
			</Container>
		</Container>
	{/each}
</Container>


