<script lang="ts">
    import Check from "lucide-svelte/icons/check";
    import ChevronsUpDown from "lucide-svelte/icons/chevrons-up-down";
    import * as Command from "$lib/components/ui/command/index.js";
    import * as Popover from "$lib/components/ui/popover/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import { cn } from "$lib/utils.js";
    import { tick } from "svelte";
    import { goto } from "$app/navigation";

    const frameworks = [
        {
            value: "projects",
            label: "Projects",
        },
        {
            value: "about",
            label: "About",
        },
        {
            value: "resume",
            label: "Resume",
        },
        {
            value: "contact",
            label: "Contact",
        },
    ];

    let open = false;
    let value = "";

    $: selectedValue =
        frameworks.find((f) => f.value === value)?.label ?? "Select...";

    // We want to refocus the trigger button when the user selects
    // an item from the list so users can continue navigating the
    // rest of the form with the keyboard.
    function closeAndFocusTrigger(triggerId: string) {
        open = false;
        tick().then(() => {
            document.getElementById(triggerId)?.focus();
        });
    }
</script>

<Popover.Root bind:open let:ids>
    <Popover.Trigger asChild let:builder>
        <Button
            builders={[builder]}
            variant="outline"
            role="combobox"
            aria-expanded={open}
            class="w-[200px] justify-between"
        >
            {selectedValue}
            <ChevronsUpDown class="ml-2 h-4 w-4 shrink-0 opacity-50" />
        </Button>
    </Popover.Trigger>
    <Popover.Content class="w-[200px] p-0">
        <Command.Root>
            <Command.Group>
                {#each frameworks as framework}
                    <Command.Item
                        value={framework.value}
                        onSelect={(currentValue) => {
                            value = currentValue;
                            closeAndFocusTrigger(ids.trigger);
                        }}
                    >
                        <a
                            href="/{framework.value}"
                            class="flex items-center w-full"
                        >
                            <Check
                                class={cn(
                                    "mr-2 h-4 w-4",
                                    value !== framework.value &&
                                        "text-transparent"
                                )}
                            />
                            {framework.label}
                        </a>
                    </Command.Item>
                {/each}
            </Command.Group>
        </Command.Root>
    </Popover.Content>
</Popover.Root>
