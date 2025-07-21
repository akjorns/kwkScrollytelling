<script>
  import { onMount } from 'svelte';

  // Track vertical scroll position
  let scrollY = 0;

  // Control sticky state of character boxes
  let sticky = false;

  // Control which character is active
  let jennyActive = false;
  let graceActive = false;

  // References to DOM elements
  let charactersRef;
  let containerRef;

  // Store the top offset of the scene container
  let sceneTop = 0;
  let naturalTop = 0;

  // Handle scroll event, update states
  const onScroll = () => {
    scrollY = window.scrollY;

    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + scrollY;
    }

    // Activate sticky based on scene's top position
    sticky = scrollY >= sceneTop && scrollY < sceneTop + 500;

    // Activate Jenny dialog between +100 and +300 px within sticky
    jennyActive = sticky && scrollY >= sceneTop + 100 && scrollY < sceneTop + 300;

    // Activate Grace dialog between +300 and +500 px within sticky
    graceActive = sticky && scrollY >= sceneTop + 300 && scrollY < sceneTop + 500;
  };

  // Setup scroll listener and initial measurements
  onMount(() => {
    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + window.scrollY;
      naturalTop = sceneTop;
    }

    window.addEventListener('scroll', onScroll);
    onScroll();

    return () => window.removeEventListener('scroll', onScroll);
  });
</script>

<style>
  /* Overall scene container styling */
  .scene {
    min-height: 75vh;
    display: flex;
    justify-content: center;
    align-items: start;
    padding-top: 150px;
    background: #034c36;
    color: white;
    font-family: monospace;
  }

  /* Container around characters, controls sticky positioning */
  .characters-container {
    position: relative;
  }

  /* Flex container for character boxes */
  .characters {
    display: flex;
    gap: 4rem;
    position: relative;
  }

  /* Individual character box styling */
  .character-box {
    background: #DAA6D3;
    padding: 1rem;
    border-radius: 4px;
    text-align: center;
    position: relative;
    width: 150px;
    transition: all 0.3s ease;
  }

  /* Invisible placeholder to keep vertical space when sticky */
  .sticky-placeholder {
    height: 200px;
    width: 100%;
    visibility: hidden;
  }

  /* Sticky positioning styles applied to container */
  .sticky .characters {
    position: fixed;
    left: 50%;
    transform: translateX(-50%);
    z-index: 10;
  }

  /* Highlight active character */
  .active {
    outline: 3px solid white;
  }

  /* Dialog bubble styling */
  .dialog {
    background: #FAD9F9;
    color: black;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    position: absolute;
    top: 110%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    box-shadow: 0 2px 6px rgba(0,0,0,0.2);
  }

  /* Smaller font for ASCII art */
  .character-box pre {
    font-size: 0.1rem;
    line-height: 1.2;
  }
</style>

<div class="scene">
  <!-- Container with sticky state toggling -->
  <div class="characters-container {sticky ? 'sticky' : ''}" bind:this={containerRef}>

    <!-- Placeholder div to preserve layout during sticky -->
    {#if sticky}
      <div class="sticky-placeholder"></div>
    {/if}

    <!-- Characters flex container, top position changes if sticky -->
    <div
      class="characters"
      bind:this={charactersRef}
      style={sticky ? `top: ${Math.max(150, naturalTop - scrollY)}px` : ''}
    >
      <!-- Jenny character box -->
      <div class="character-box {jennyActive ? 'active' : ''}">
        <pre>
                                                                                                            
                                         @@@@  @       @@@@@@@                                   
                                       #@*=@@@@                  @@@@                               
                                     @@  @@@      @@@@@@@@ @@@      @@@@                            
                                   %#  @@    @@@@          @@@@ @@@@   @@@                          
                                  =:  @@   @@     @@            @@  @@@  @@@                        
                                 :.   @ @@@  @@@              @    @   @@@ @@                       
                         @@@@@@@%@ @@@@ @@  @  @@@@ @@@@@        @@      @@  @                      
                       @@@ @@  +@@    @@@@ @ @@          @@         @@     @@ @*                    
                     @@@@@@  @@   @@  @@@@  @@ @@@@@@@      @   @     @@    @@ @                    
                    @@ %+  @@    @@  @@@    @@        @@@@   @@  @@    @@:   @@ @                   
                   @@@#  *#    @ @@@                      @@@  @@  @@   :@    @@@@                  
                  @@@@  ..     @ @@                         @@   @  @@    +    @@@                  
                 @@@@        @@ @@                            @@  @@  @@  +@%   @@                  
                @@-%       @@ @@@@                              @   +  @@  @@   @@                  
                #++-    @@   @@@@                                @.      @  @@  @@                  
                % @   @@    @ @@@                                  -      @ @@@ @@                  
                @ @ @@     @@@@@    @@@@@@@            @@@@@@@@@%   @      @@@@@@@                  
                @@  @    @@@@@ @@@@@@@@@@@@@          @@@@@      @@@@@@     @@@@@@                  
               @@@ @@ @@@@@ @       @                        + %      @@    @@@@@@                  
               @@@ @ @@@@@@@@  @@@@@@@@@@@               @%#@@@@@@ @  @@@   @@@@@                   
               @ @ @@@@@@@@@@ @@@@@@@@@@@@@@            @@@@@@@@@@@@@@@@@@  @@@@                    
               @  @ @@@@  @@@ @@@@ @ @@@@@@@            @@@@@@@@@ @   @@@@@ @@@@                    
                @  @ @@@ @@ @   @@ @@   @@ @           @  @@  @@ @@   @@ @@@@@@                     
                 @@ @ @@@ @ @  @ @@@@@@@@@               @@@@@@@@@    @@@@@ @@                      
                  @@@  @@@ @@@                                        @@@ @@@                       
                    @@@ @@@ @@                                        @@@@@@ @                      
                       @@@  @@                    @                  @   @    @@                    
                        @@@@  @                                     @@ @@ @@   @@                   
                         @@ @@@@             @@@@  @@@             @@@@ @   @@  @@                  
                          @    @                @                 @@@   @@   @@  @                  
                            @  @@                                 @ @@   @    @@ @                  
                             @  @@            @@@@@@@@           @@@  @   @    @@@                  
                              @  %@%       @@@@ @@@@  @@        @@ @@ @@  @@   @@@  %               
                               @   @@         @      @        @@    @  @  @@  @@@@  *@              
                   @@          @:   @@@          @@         @@      @  @  @  @@ *@  @@              
                 @@@@          %@  @  @@@                 @@        @@@  @ @@   #* @@               
                @@@@           @  @@  @@  @             @@ @      @@@  @@@@    @@@@@                
                @@@@          @@ @@   @@    @@@      @@@   @      @@  @@     #@@@@                  
                @@@@         @@@@     @@       @@@@@@      @      @ @@  =#%@#*@                     
                 @@@@@%   :@@@@@      @@                   @@     @@  @@  ..                        
                    @@@@@@@@@        @@                    @@     @@@  @  @                         
                                     @@                     @@    @%@#@@@@   @                      
                                   @@@                       @@    %@       @@                      
                                 @@@                           @%    @@@@@@@@                       
                                @                                @@                                 
                           @@@@                                    @@@                              
                      @@@@@ @@                                        @@@@                          
                  @@@@   @   @@                                      @  @@ @@@@                     
               @@@       @@   @@                                    @@   @     @@@                  
              @           @    @@                                  @@    @         @@               
                                 @@                               @                   @             
                                   @                                                                                 
                                                                                                                          
        </pre>
        <div><strong>Jenny</strong></div>
        {#if jennyActive}
          <div class="dialog">Jenny goes to her local public school.</div>
        {/if}
      </div>

      <!-- Grace character box -->
      <div class="character-box {graceActive ? 'active' : ''}">
        <pre>
                                                                                                                
                                                                                                                 
                                                                                                    
                                                                                                    
                                             @@@@     @@@@@                                         
                                      @@%*%@@@@@@          @@@@@                                    
                                 @@@@      @                   @@@@                                 
                               @@   -#%@@@@@@@@@@@                @@@                               
                             @@@  %#  @@@@@@@@@@  @@@               @@@                             
                            @@ @%* :*@           @@@ @@@  @           @@                            
                           @@ @%         @@         @@@ @@@ @@         @@                           
                          @@@@         @@@@            @@ @@@ @@        @@                          
                         @@ @        @@   @@@               @@@ @@   @   @@                         
                        .%-@        @@    @@@@                @@ @@  @@   @#                        
                        *@#:       @@@    @@@@@                 @@ @  @   +@                        
                        *#=#      @@@      @@@@@      @  @       @@ @ @@   #*                       
                        @@@@      @@       @@@ @@      @  @@       @ @@@   +@                       
                       @@@@   @   @@        @@@@@@ @@   @   @       %#@@    @@                      
                       @@@   @@  @@          @@@@@@@ @@  @@  @@   .  %@@    @@                      
                       @@@   @ @@@   @@@@@@   @@  @@@@@@@@@@@@@@%  +@ @@    @@                      
                       @@   @  @@@@@@@@@@@@     @@   @@@@@  @@%@@%@  @@@ @@  @                      
                      @@@ @@ @@@@                @@@       @# *@@@@@@@@@@ @@@@@                     
                      @@@@@@@@@    @@@@@@           @@ @@@@@@@@@@    @ @@@@@ @@                     
                      @@@@@@@@   @@@@@@@@@@            @@%@@@@@@@@@  @@@ @@@@ @@                    
                      @@@@ @@@ @@@ @@@@@@@              @%@@@ @ @@@ @@    @@@ @@                    
                      @@@@  @@  @@@@@ @ @@              @@@@@@ @@   @@@  @@@@ @@                    
                      @@@@@ @@@     @@@@@@                          @@  @@ @@  @                    
                       @  @@ @@                                     @  @@  @   @@                   
                     @@@    @@@@                @                  @@@@@@ @@    @                   
                     @@       @@                                   @@@ @  @@    @@                  
                    @ @       @@@             @@  @               @@@ @@ @@     @@                  
                   @@@@        @@@                                @@ @@ @@      @@@                 
                   @ @@     @  @@@@       =@          @@@       @@@@@@  @       @@@                 
                  @@ @@     @  @  @@        @@@@@@@@@@         @@@@@@  @    @  @@@@                 
                  @@ @@    @@  @   @@@                       @@@@@@@  @   @@  @@@@                  
                  @@@ @    @  @@     *@@@        @@       @@@@@ @@@  @  @@    @ @@                  
                  @@@ @@   @  @       @ @@@@            @@@ @@@ @@  @@ @@   @@ @@@                  
                   @@@ @@  @@ @       @ @@@@@@@      @@@  @@@ @ @@  @ @@  @@ @@@                    
                    @@@ @@  @ % -    @  @@@@  @@@@@@@@    @@@@@@@@@ @@@@@@@@@@                      
                      @@@@@@@@*  @@@@  @@@@@              @@@@@@@@@@@@@@                            
                            @@@@@@@@@@@  @@               @@                                        
                                        @@                 @@@                                      
                                      @@@    @         @     @@@                                    
                                 @@@@@@      @@        @        @@@@                                
                               @@ @@          @@                @@ @@@@                             
                            @@@@@ @@@                         @@@@ %   @@@@@                        
                       @@@@@    @@ @@@@@@@@               @@@ @@  %+        @@@@@                   
                   @@@@@@         @ @@@          @@@@@       @@ .@*         @@  @@@                 
                  @@    @@         @@ @@@                  @@  =@          @@     @@@               
                @@       @           @@  @@@@          @@@@  %@=           @        @@              
               @@        @@             @@@  @@@@@@@@@@   @@@             @@         @@             
              @@          @                 @@@@@@@@@@@@#                 @@          @@            
              @           @                                               @            @            
                                                                                                    
                                                                                                                                                    
                                                                                                    
        </pre>
        <div><strong>Grace</strong></div>
        {#if graceActive}
          <div class="dialog">So does Grace.</div>
        {/if}
      </div>
    </div>
  </div>
</div>
