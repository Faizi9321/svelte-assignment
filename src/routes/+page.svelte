<script lang="ts">
	import SveltyPicker from 'svelty-picker';
	import { Button, Text } from '@svelteuidev/core';
	import { Plus, Cross1 } from 'radix-icons-svelte';
	import { Container } from '@svelteuidev/core';
    import { slide } from 'svelte/transition';
    import '../style.scss'

	let days: any[] = [
		{ day: 'Monday', timeSlots: [{ openTime: '', closeTime: '' }] },
		{ day: 'Tuesday', timeSlots: [{ openTime: '', closeTime: '' }] },
		{ day: 'Wednesday', timeSlots: [{ openTime: '', closeTime: '' }] },
		{ day: 'Thursday', timeSlots: [{ openTime: '', closeTime: '' }] },
		{ day: 'Friday', timeSlots: [{ openTime: '', closeTime: '' }] },
		{ day: 'Saturday', timeSlots: [{ openTime: '', closeTime: '' }] },
		{ day: 'Sunday', timeSlots: [{ openTime: '', closeTime: '' }] }
	];

    const addMore = (item:string) => {
        console.log(item);
        days.forEach((element)=>{
            if(element.timeSlots[0] == 'Closed'){
                console.log('close');
                element.timeSlots = []
                element.timeSlots.push({ openTime: '', closeTime: ''})
            }else if(element.day == item){
                element.timeSlots.push({ openTime: '', closeTime: ''})
            }
        })
        days = days
    }

    const onRemove = (item:string,index:number) => {
        days.forEach((element)=>{
            if(element.day == item){
                element.timeSlots.splice(index,1)
            }
            if(element.timeSlots.length == 0){
                element.timeSlots.push('Closed')
            }            
        })
        days = days
    }
</script>

<Container override={{ dflex: 'center', fd: 'column', my: '80px' }}>
	{#each days as item}
		<Container override={{ d: 'flex', jc:'center', ac:'start', mt: '10px' }}>
			<Container override={{ mx: '10px',mt: '11px',w: '100px', textAlign: 'center' }}>
				 <Text>{item.day}</Text>
			</Container>
			<Container override={{d:'flex',fw:'wrap',w:'264px',pr:'0px'}}>
                {#if item.timeSlots[0] == 'Closed'}
                    <Container override={{px: '24px', py: '11px'}}>
                        <Text>{item.timeSlots[0]}</Text>
                    </Container>
                    <Container override={{ dflex: 'center',px: '0px' }}>
                        <Button override={{mx:'6px',w: '40px',px: '12px'}} on:click="{() => addMore(item.day)}">
                            <Plus />
                        </Button>
                        <Button override={{mx:'6px',bgColor:'Silver',w: '40px',px: '12px'}} on:click="{() => onRemove(item.day,0)}" disabled={true}>
                            <Cross1 />
                        </Button>
                    </Container>
                {:else if item.timeSlots.length > 0}
                    {#each item.timeSlots as hours, index}
                        <div class="d-flex" in:slide="{{ axis: 'y', duration: 1000 }}">
                            <Container override={{my:'4px',px: '0px',py: '0px'}} >
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
                            <Container override={{ dflex: 'center',px: '0px' }}>
                                <Button override={{mx:'6px',w: '40px',px: '12px'}} on:click="{() => addMore(item.day)}">
                                    <Plus />
                                </Button>
                                <Button override={{mx:'6px',bgColor:'Silver',w: '40px',px: '12px'}} on:click="{() => onRemove(item.day,index)}">
                                    <Cross1 />
                                </Button>
                            </Container>
                        </div>
                    {/each}
                {/if}
			</Container>
		</Container>
	{/each}
</Container>


