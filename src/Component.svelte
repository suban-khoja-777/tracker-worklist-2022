<script>

    import { fireEvent, EVENTS } from "./EventManager";
    import { PAYMENT_STATUS ,getEntryPaymentStatusClass , formatDateString , formatTimeString, ENTRY_DURATIONS } from "./constants";
    import Input from "./utility/Input.svelte";
    import Icon from "./utility/Icon.svelte";
    export let entry;
    export let actions = [];

    const sendOpenComponentDetailEvent = () => {
        fireEvent(EVENTS.OPEN_EDIT_ENTRY_POPUP,entry._id);
    }

    const sendChangeStatusEvent = (evt) => {
        entry.Payment_Status =  evt.target.value;
        fireEvent(EVENTS.UPDATE_ENTRY_STATUS,{
            _id : entry._id,
            Payment_Status : entry.Payment_Status
        });
    }

    const sendChangeDurationEvent = (evt) => {
        entry.Duration =  evt.target.value;
        fireEvent(EVENTS.UPDATE_ENTRY_DURATION,{
            _id : entry._id,
            Duration : Number(entry.Duration)
        });
    }

    const sendDeleteComponentDetailEvent = () => {
        fireEvent(EVENTS.DELETE_ENTRY,entry._id);
    }
    
    const sendViewComponentDetailEvent = () => {
        fireEvent(EVENTS.OPEN_VIEW_ENTRY_POPUP,entry._id);
    }

</script>

<li class="columns flex justify-space-between align-center border-box">

    <span class="column flex grow justify-center border-box text-bold pointer entry_column justify-center align-center">
        <Input label="Name"  width_class="width-full" type="text" classes="bg-transparent" value={formatDateString(entry.Start_Date)} data_field="start_date" readOnly/>
        <span class="row-actions">
            {#if actions.includes('EDIT')}
                <Icon type="warning" OnClick={sendOpenComponentDetailEvent}>
                    <svg xmlns="http://www.w3.org/2000/svg" width="20px" height="20px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-75 feather feather-edit-2">
                        <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                    </svg>
                </Icon>
                &nbsp;&nbsp;
            {/if}
            {#if actions.includes('DELETE')}
                <Icon type="danger" OnClick={sendDeleteComponentDetailEvent}>
                    <svg xmlns="http://www.w3.org/2000/svg" width="20px" height="20px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-75 feather feather-trash">
                        <polyline points="3 6 5 6 21 6"></polyline>
                        <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path>
                    </svg>
                </Icon>
            {/if}
            {#if actions.includes('VIEW')}
                <Icon type="warning" OnClick={sendViewComponentDetailEvent}>
                    <svg data-v-9a6e255c="" xmlns="http://www.w3.org/2000/svg" width="20px" height="20px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" id="invoice-row-5036-preview-icon" class="mx-1 feather feather-eye" style="">
                        <path data-v-9a6e255c="" d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                        <circle data-v-9a6e255c="" cx="12" cy="12" r="3"></circle>
                    </svg>
                </Icon>
                &nbsp;&nbsp;
            {/if}
        </span>

    </span>
    <span class="column flex grow justify-center border-box text-bold pointer entry_column  justify-center align-center">
        <Input label="Start Time" width_class="width-full" type="text" classes="bg-transparent text-center" value={formatTimeString(entry.Start_Time)} data_field="Start_Time" readOnly/>
    </span>
    <span class="column column-select flex grow justify-center border-box text-bold pointer entry_column  justify-center align-center">
        <Input label="Duration" width_class="width-full" type="select" classes="bg-transparent text-center border-radius-5 height-100" value={Number(entry.Duration)} data_field="Duration" options={ENTRY_DURATIONS} onChange={sendChangeDurationEvent}/>
    </span>
    <span class="column column-select flex grow justify-center border-box text-bold pointer entry_column justify-center align-stretch">
        <Input label="Payment Status" width_class="width-full" type="select" classes="bg-transparent text-center border-radius-5 {getEntryPaymentStatusClass(entry.Payment_Status)}" value={entry.Payment_Status} data_field="Payment_Status" options={PAYMENT_STATUS} onChange={sendChangeStatusEvent}/>
    </span>
    <span class="column flex grow justify-center border-box text-bold pointer entry_column  justify-center align-center">
        <Input label="Amount" width_class="width-full" type="text" classes="bg-transparent text-center text-boldest" value="${entry.Amount}" data_field="Amount" readOnly/>
    </span>
         
</li>

<style>

    .columns{
        width: 100%;
        list-style: none;
        background-color: transparent;
        margin-bottom: 4px;
        color: #3F4254;
        border-bottom: 1px solid #EFF2F5;
    }

    .column-select{
        padding: 5px 25px;
    }

    .column{
        height: 40px;
    }

    .row-actions{
        padding: 0 1rem;
        display: none;
    }

    li.columns:hover .row-actions{
        display: inherit;
    }

    .columns:last-of-type{
        border-bottom: 0;
    }


</style>