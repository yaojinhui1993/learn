# Frontend

## Blade Templates

### ViewTest

- testDataCanBeSetOnView
- testRenderProperlyRendersView
- testRenderHandlingCallbackReturnsValues
- testRenderSectionsReturnReturnEnvironmentSections
- testSectionsAreNotFlushedWhenNotDoneRendering
- testViewNestBindASubView
- testViewAcceptsArrayableImplementations
- testViewGettersSetters
- testViewArrayAccess
- testViewConstructedWithObjectData
- testViewMagicMethods
- testViewBadMethod
- testViewGatherDataWithRenderable
- testViewRenderSections
- testWithErrors

### ViewBladeCompilerTest

- testIsExpiredReturnsTrueIfCompiledFileDoesntExist
- testCannotConstructWithBadCachePath
- testIsExpiredReturnsTrueWhenModificationTimesWarrant
- testCompiledPathIsProperlyCreated
- testCompileCompilesFileAndReturnsContents
- testCompileCompilesAndGetThePath
- testCompileSetAndGetThePath
- testCompileWithPathSetBefore
- testRawTagsCanBeSetToLegacyValues

### ViewCompilerEngineTest

- testViewsMayBeRecompiledAndRendered
- testViewsAreNotRecompiledIfTheyAreNotExpired