<div class="repo-box nes-container with-title m-12">
    <h1 class="title nes-text is-success">
        <slot name="title">
            <span class="missing">Unknown Name</span>
        </slot>
    </h1>
    <p>
        <slot name="description">
            <span class="missing">Unknown Description</span>
        </slot>
    </p>
    <div class="nes-badge">
        <span class="is-dark">
            <slot name="language">
                <span class="missing">Unknown Language</span>
            </slot>
        </span>
    </div>
    <button type="button" class="nes-btn is-success">
        <slot name="button">
            <span class="missing">Unknown Button</span>
        </slot>
    </button>
</div>