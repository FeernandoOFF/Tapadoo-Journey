<script>
    import 'aframe'
    import 'aframe-mouse-cursor-component'
    import {onMount} from "svelte";
    const scenes = [
        {
            sceneImage: '#entrance',
            nextSceneIndicators: [
                {
                    x: 10,
                    y: 0.4,
                    z: -2,
                    rotation: -95,
                    to: 0,
                    locationName: 'Kitchen'
                }

            ]
        },
        {
            sceneImage: '#kitchen',
            nextSceneIndicators:
                [
                    {
                x: 4,
                y: 0.2,
                rotation: 0,
                z: -9,
                to: 1,
                locationName: 'Boss room'
            }]


        },
        {
            sceneImage: '#boss_room',
            nextSceneIndicators:
                [
                    {
                        x: 6.4,
                        y: 1.4,
                        rotation: -45,
                        z: -9,
                        to: 3,
                        locationName: 'The bati-cave'
                    },
                ]
        },
        {
            sceneImage: '#office',
            nextSceneIndicators:[
                {
                    x: 0,
                    y: 0.2,
                    z: 10,
                    to: 1,
                    rotation: 180,
                    locationName: 'Boss room'

                }

            ]
        },
        {
            sceneImage: '#main_entrance',
            nextSceneIndicators:[
                {
                    x: 8.3,
                    y: 1.2,
                    z: -3.70,
                    to: 2,
                    rotation: -50,
                    locationName: 'Boss room'

                }
            ]
        },


    ]

    let currentSceneIndex = 0
    $: currentScene = scenes[currentSceneIndex]
    let navigating = false
    function navigateToScene(index){
        if(navigating) return
        navigating = true
        setTimeout(()=>navigating = false,1000)

        if(currentSceneIndex >= scenes.length -1) {
            currentSceneIndex = index
            return
        }

        currentSceneIndex = currentSceneIndex + 1

    }
</script>

<a-scene cursor__mouse="rayOrigin: mouse"
         cursor__xrselect="rayOrigin: xrselect"
         raycaster="objects:#objects *;" >

    <a-assets>
        <img id="entrance" src="/tapadoo.jpg">
        <img id="kitchen" src="/kitchen.jpg">
        <img id="office" src="/office.jpg">
        <img id="arrow" src="/arrow.png">
        <img id="main_entrance" src="/main_entrance.jpg">
        <img id="boss_room" src="/boss_room.jpg">
    </a-assets>

    <a-sky src="{currentScene.sceneImage}" rotation="0 -80 0"></a-sky>
<!--  Controls  -->
    <a-camera>
        <a-cursor
                id="cursor"
                animation__click="property: scale; from: 0.3 0.3 0.3; to: 1 1 1; easing: easeInCubic; dur: 150; color: red; startEvents: click"
                animation__fusing="property: scale; from: 1 1 1; to: 0.1 0.1 0.1; easing: easeInCubic; dur: 150; startEvents: fusing">

        </a-cursor>
    </a-camera>

<!--    Scene -->

    {#each currentScene.nextSceneIndicators as navigation}

        <a-entity
                rotation="0 {navigation.rotation} 0"
                position="{navigation.x} {navigation.y} {navigation.z}"
                cursor-listener
                on:mouseenter={()=>navigateToScene(navigation.to)}
        >

            <a-plane
                color="#CCC"
                height="2.5" width="3"
                position="0 0 -0.1">
            </a-plane>

            <a-text
                    value="{navigation.locationName}"
                    position="0 -1 0"
            ></a-text>

            <a-image
                    src="#arrow"
            ></a-image>

        </a-entity>

    {/each}

<!--    <a-text font="kelsonsans" value='"A day at Work"' width="6" position="-2.5 0.25 -1.5" />-->
<!--    <a-text value='Masterpiece done by Fernando Obregon' width="6" position="-2.5 0.25 -1.5"/>-->
<!--    <a-text  value='by Avoiding to learn Android stuff' width="6" position="-2.5 0.25 -1.5"-->
<!--            rotation="0 15 0"></a-text>-->


</a-scene>